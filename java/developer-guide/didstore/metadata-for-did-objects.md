# Metadata of the DID objects

For the convenience of application development, DID Store provides the ability to save some local data for DID objects stored within, which is called the metadata of DID objects by DID SDK.

Different DID object implementations provide different metadata data according to their own needs - typical examples are DID Metadata and Credential Metadata. Both DID and verifiable credentials can get their metadata interface from the object and access related data.&#x20;

For example:

```java
DIDStore store; // an opened DIDStore instance
DID did = new DID("did:elastos:iU7yMyvoqEqJQgjX71W627whs1MGmEM5j3");
DIDURL vcId = new DIDURL("did:elastos:iU7yMyvoqEqJQgjX71W627whs1MGmEM5j3#profile");

DIDDocument doc = store.load(did);
DIDMetadata dm = doc.getMetadata();
dm.setAlias("Primary DID");
// ...

VerifiableCredential vc = store.loadCredential(vcId);
CredentialMetadata cm = vc.getMetadata();
String alias = cm.getAlias();
// ...
```

The interface of metadata may be different in SDKs implemented in different languages, so it can be used reasonably according to specific SDKs.

In addition, Root Identity and DID Store also use the Metadata mechanism provided by the store, only because of the need of implementation. There is no metadata interface provided directly, but the local data of their objects will also use metadata.
