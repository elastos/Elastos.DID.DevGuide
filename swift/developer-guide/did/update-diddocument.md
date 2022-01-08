# Modify and update the DID Document

DIDDocument is a sealed object which cannot be directly modified. The DID SDK provides a mechanism to modify the DID document. According to this mechanism, we can create a modifiable copy based on the original document, modify, and re-sign this document to generate a new document instance. The DID controller can update the DID document by publishing it in the updated mode - see [Publish DID](publish-did.md) for the detailed publishing process.

In the implementation of Swift SDK, the editing of DID documents is realized through the DIDDocumentBuilder object. The reference example is as follows:

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let did = try DID("did:elastos:ihmtZkypEiJZHv7sKBXmkHQDYVPp1ACgkZ")
// Get the existing DIDDocument
let doc = try store.loadDid(did)
// Create document builder and a copy for editing
let db = doc.editing()
// Do some modification on the document
db.addService("#vcr", "CredentialRepositoryService",
				"https://example.com/credentials")
// ...
// other modifications
// Seal the document
let newDoc = db.seal(using: storePasswd)
```

The customized DID can be edited in a similar way, but only the valid controller of DID can edit the document. After editing the multi-signed DID, a process similar to that of a multi-signed customized DID should be created and adopted to sign the DIDdocument.

DIDDocumentBuilder offers a series of methods to modify and manage the content of DID documents.
