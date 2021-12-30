# Access DIDStore

DID Store provides a series of reading and writing methods for stored objects, which can be divided into the following categories:

* The store series method saves the DID object
* The load series method reads the DID object
* The contains series method tests whether the DID object is stored in the store
* List series method list specific DID object set
* The delete series method deletes the DID object

Ordinarily, only the document and Private Key of personal DID, as well as the associated verifiable credentials, are kept in DID Store. DID Document objects of others can be parsed and obtained from the ID sidechain and do not need to be saved in DID Store.

## Related Operation of DID

```
DIDStore store; // an opened store instance
```

```java
DIDDocument doc; // a DIDDoucment instance

// Save the DID document to the store
store.storeDid(doc);

// Load the DID document from the store
DIDDocument myDoc = store.loadDid("did:elastos:ihQQhyf6PW4YEc8mRi8AWmitCcMxz5kiBq");

// Check the store has this DID document
boolean exists = store.containsDid("did:elastos:ihQQhyf6PW4YEc8mRi8AWmitCcMxz5kiBq");

// Check the store has any DIDs exist
boolean exists = store.containsDid();

// Delete the specific DID
store.deleteDid("did:elastos:ihQQhyf6PW4YEc8mRi8AWmitCcMxz5kiBq");

// List all DIDs in the store
List<DID> dids = store.listDids();
// Or list DIDs with a customized filter
List<DID> dids = store.listDids((did) -> {
        // return true if include this DID, otherwise return false.
        return true;
    });
```

## Related Operation of Verifiable Credentials

```java
DIDStore store; // an opened store instance
VerifiableCredential vc; // a VerifiableCredential instance

// Save the credential to the store
store.storeCredential(vc);

// Load the credential from the store
VerifiableCredential myVc = store.loadCredential(
    "did:elastos:ihQQhyf6PW4YEc8mRi8AWmitCcMxz5kiBq#profile");

// Check the store has this credential
boolean exists = store.containsCredential(
    "did:elastos:ihQQhyf6PW4YEc8mRi8AWmitCcMxz5kiBq#profile");

// Check the store has any credential exists for specific DID
boolean exists = store.containsCredentials(
    "did:elastos:ihQQhyf6PW4YEc8mRi8AWmitCcMxz5kiBq");

// Delete the specific credential
store.deleteCredential(
    "did:elastos:ihQQhyf6PW4YEc8mRi8AWmitCcMxz5kiBq#profile");

// List all credentials for specific DID in the store
List<DIDURL> vcIds = store.listCredentials(
    "did:elastos:ihQQhyf6PW4YEc8mRi8AWmitCcMxz5kiBq");
// Or list credenitals with a customized filter
List<DIDURL> vcIds = store.listCredentials(
    "did:elastos:ihQQhyf6PW4YEc8mRi8AWmitCcMxz5kiBq", (vcId) -> {
        // return true if include this credential, otherwise return false.
        return true;
    });
```

## Related Operations of Root Identity <a href="#access-rootidentity" id="access-rootidentity"></a>

## Related Operations of Private Key

```java
DIDStore store; // an opened store instance
byte[] privateKey; // the plain binary private key
DIDURL keyId = new DIDURL("did:elastos:ihQQhyf6PW4YEc8mRi8AWmitCcMxz5kiBq#official");
String storePasswd = "secret"; // should be your store password

// Save the private key to the store
store.storePrivateKey(keyId, privateKey, storePasswd);

// Check the private key exists in the store
boolean exists = store.containsPrivateKey(keyId);

// Delete the specific private key
store.deletePrivateKey(keyId);
```
