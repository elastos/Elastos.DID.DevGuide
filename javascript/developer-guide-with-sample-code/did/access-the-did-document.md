# Access the DID Document

The DID document provides the method to get the number and individuals of internal objects, i.e. Subject, Controller, Multisig, Public Key, Authentication Key, Authorization Key, Verifiable Credential, and Service. It's relatively simple to understand and use these APIs, and users can refer to the API document directly.

This section mentions the method provided by the DID document to acquire DID Metadata and Default Key.

## Usage

```typescript
public getDefaultPublicKeyId(): DIDURL；
```

This method is the main key to get DID document.

```typescript
public getMetadata(): DIDMetadata；
```

This method gets DIDMetadata, which contains the information that the DID cannot put into DID document.
