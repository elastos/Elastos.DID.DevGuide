# Access the DID Document

DID Document provides get method to get the number and individuals of internal objects: Subject, Controller, Multisig, Public Key, Authentication Key, Authorization Key, Verifiable Credential, and Service. These APIs are relatively simple to understand and use, and the API documents can be referred to directly.

This section mentions the methods provided by DID Document to get DID Metadata and Default Key.

## Usage

```c
DIDURL *DIDDocument_GetDefaultPublicKey(DIDDocument *document)；
```

The method is to get the main Key of DID Document.

```c
DIDMetadata *DIDDocument_GetMetadata(DIDDocument *document);
```

This method gets DIDMetadata, which contains the information that DID cannot put into DID Document.

```c
```
