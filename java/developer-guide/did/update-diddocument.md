# Modify and Update the DID Document

DID Document is a sealed object and cannot be directly modified. The DID SDK provides a mechanism to modify it, which can create a modifiable copy based on the original document, and meanwhile, re-sign the document and generate a new document instance. The DID controller can publish this DID document in the updated mode, thus realizing the update of DID Document. See DID publishment for publishing process.

In the implementation of Java SDK, DID document edit is realized by DIDDocument.Builder object. Example:

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID did = new DID("did:elastos:ihmtZkypEiJZHv7sKBXmkHQDYVPp1ACgkZ");

// Get the existing DIDDocument
DIDDocument doc = store.loadDid(did);

// Create document builder and a copy for editing
DIDDocument.Builder db = doc.edit();

// Do some modification on the document
db.addService("#vcr", "CredentialRepositoryService",
        "https://example.com/credentials");
// ...
// other modifications

// Seal the document
DIDDocument newDoc = db.seal(storePasswd);
```

Editing of customized DID is similar, but it needs the valid controller of DID to enable this. After editing the multi-signed DID, the DID document needs to be signed by [creating a multi-signed customized DID](create-multi-signed-customized-did.md).

DIDDocument.Builder object provides a series of methods to modify and manage the contents of DID documents. See Javadoc for details.
