# Publish DID

When DID is used as an identity identifier, it's usually necessary to publish the DID first so that it can be parsed and verified conveniently by the respective party. The publishing operation of DID includes initial publication creation and publication update.

* Create- used for newly created DID, which is published to ID side chain for the first time.
* Update- used to update previously published DID.

The DID SDK will automatically construct the corresponding ID transaction according to the chain state of DID. The constructed ID transaction needs to be signed with the valid DID authentication key, which means that only the controller of DID can publish DID.&#x20;

Here's an example of creating a new DID and publishing it:

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
RootIdentity identity; // a RootIdentity instance

// Get the new created DIDDocument
DIDDocument doc = identity.newDid(storePasswd);

// Publish the DID to the ID side chain using the default key
doc.publish(storePasswd);
```

When publishing DID, the default is to use the regular authentication key corresponding to DID to sign. In the above example, the default key is used to sign the transaction - this is also the pattern used by most applications. If a DID has set multiple authentication keys, and it is necessary to specify a specific authentication key to sign the transaction, then the ID of the target key can be specified in the publish method, and the corresponding Private Key is required in the DID Store where DID Document is located.&#x20;

Example:

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";

DIDDocument doc; // the DIDDocument to be publish
// The key id should defined in the document, also the private key in the store
DIDURL signKey = new DIDURL("did:elastos:igeMDX9wRHJBL72ymp3JrS3WE7jXTAs7kn#secondary");

// Publish the DID to the ID side chain using the default key
doc.publish(signKey, storePasswd);
```

The publishing operation when updating DID is the same as that when innovating. The DID SDK will automatically process the type of transaction according to the DID status on the chain, update a published DID document object, and publish an example as follows:

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID did = new DID("did:elastos:igeMDX9wRHJBL72ymp3JrS3WE7jXTAs7kn");

// Get the existing DIDDocument
DIDDocument doc = store.loadDid(did);
DIDDocument.Builder db = doc.edit();
// Do some modification on the document
DIDDocument newDoc = db.seal(storePasswd);

// Update the existing DIDDocument on the ID side chain
doc.publish(storePasswd);
```

The publication of customized DID is similar to that of primitive DID, and the publication transaction can be signed by any controllers’ valid authentication key or user’s own declared authentication key. Example:

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
// foobar controlled by igeMDX9wRHJBL72ymp3JrS3WE7jXTAs7kn
// and the store has the default private key for igeMDX9wRHJBL72ymp3JrS3WE7jXTAs7kn
DID did = new DID("did:elastos:igeMDX9wRHJBL72ymp3JrS3WE7jXTAs7kn");
DID foobar = new DID("did:elastos:foobar");

// Get the existing DIDDocument
DIDDocument foobarDoc = store.loadDid(foobar);
// igeMDX9wRHJBL72ymp3JrS3WE7jXTAs7kn on beharf of foobar
foobarDoc.setEffectiveController(did);

// publish foobar by igeMDX9wRHJBL72ymp3JrS3WE7jXTAs7kn
foobarDoc.publish(storePasswd);
```

The publish operation in the above example is like the publishing of primitive DID. You can also specify the valid authentication key of the current controller to sign the transaction.

> Every DID document has an effective period. In principle, the DID will be invalid after the expiration of the effective period, so it's necessary to update the DID document, extend the effective period, and publish it before the expiration of the effective period. In addition, the deactivate DID is a permanently invalid DID and cannot be updated or published.

### Inside publish

The publish process of DID is to update the DID document to the EID side chain through the transaction of the EID side chain. The DID SDK does not have a built-in function for creating EID sidechain transactions. This function is completed by the DID Adapter interface provided by the developer when the DID Backend is initialized. When the developer calls publish, the DID SDK will package the DID document to be published as the payload of the EID transaction, and then hand it over to the DID Adapter when the DID Backend is initialized to complete the transaction creation and on-chain. DID Adapter can choose different implementation options given the needs of the application. The two different options are included in the sample code of the SDK:

* AssistDIDAdapter.java- reference implementation of DID Adapter based on Assist API
* Web3Adapter.java- reference implementation of DID Adapter based on Web3 API
