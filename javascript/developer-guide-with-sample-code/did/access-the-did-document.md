# Access the DID Document

DID Document提供get方法获取内部对象的数量和个体：Subject，Controller，Multisig，Public Key，Authentication Key，Authorization Key，Verifiable Credential，Service。这些API理解和使用都相对简单，可直接参考API文档即可。

The DID document provides the method to get the number and individuals of internal objects, i.e. Subject, Controller, Multisig, Public Key, Authentication Key, Authorization Key, Verifiable Credential and Service. It is relatively simple to understand and use these APIs, and users can refer to the API document directly.

这小节提一下DID Document提供的获取DID Metadata和Default Key方法。

This section mentions the method provided by the DID document to acquire DID Metadata and Default Key.

## Usage

```typescript
public getDefaultPublicKeyId(): DIDURL；
```

该方法是获取DID Document的主Key。

This method is the main key to get DID document.

```typescript
public getMetadata(): DIDMetadata；
```

该方法获取DIDMetadata，DIDMetadata里包含了DID无法放入DID Document的信息内容。

This method gets DIDMetadata, which contains the information that DID cannot put into DID document.
