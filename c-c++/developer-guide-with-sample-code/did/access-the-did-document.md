# Access the DID Document

DID Document提供get方法获取内部对象的数量和个体：Subject，Controller，Multisig，Public Key，Authentication Key，Authorization Key，Verifiable Credential，Service。这些API理解和使用都相对简单，可直接参考API文档即可。

DID Document provides get method to get the number and individuals of internal objects: Subject, Controller, Multisig, Public Key, Authentication Key, Authorization Key, Verifiable Credential, Service. These APIs are relatively simple to understand and use, and the API documents can be referred directly.

这小节提一下DID Document提供的获取DID Metadata和Default Key方法。

This section mentions the methods provided by DID Document to get DID Metadata and Default Key.

## Usage

```c
DIDURL *DIDDocument_GetDefaultPublicKey(DIDDocument *document)；
```

该方法是获取DID Document的主Key。

The method is to get the main Key of DID Document.

```c
DIDMetadata *DIDDocument_GetMetadata(DIDDocument *document);
```

该方法获取DIDMetadata，DIDMetadata里包含了DID无法放入DID Document的信息内容。

This method gets DIDMetadata, which contains the information that DID cannot put into DID Document.

```c
```
