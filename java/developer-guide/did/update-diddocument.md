# Modify and update the DID Document

DIDDocument 是 sealed 对象，不能直接进行修改。DID SDK 提供了一个修改 DID 文档的机制，可以在原有文档的基础上创建一个可修改的副本，并在完成修改的同时，重新对该文档进行签名，并生成一个新的文档实例。DID持有人可以用更新的模式发布这个 DID 文档，从而实现 DIDDocument 的更新。发布过程参见[发布DID](publish-did.md)。

DID Document is a sealed object and cannot be modified directly. The DID SDK provides a mechanism to modify the DID document, which can create a modifiable copy based on the original document, and meanwhile, re-sign the document and generate a new document instance. The DID controller can publish this DID document in the updated mode, thus realizing the update of DID Document. See DID publishment for publishing process.

在 Java SDK 的实现中，编辑 DID 文档是通过 DIDDocument.Builder 对象来实现的。参考示例如下：

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

自定义 DID 的编辑类似，但是需要 DID 的有效持有人身份才能进行编辑；多签的 DID 编辑完成后需要采用和[创建多签自定义 DID](create-multi-signed-customized-did.md) 中类似的流程对 DIDDocument 进行签名。

The editing of customized DID is similar, but it needs the valid controller of DID to edit; After editing the multi-signed DID, the DID document needs to be signed by a process like that in creating a multi-signed customized DID.

DIDDocument.Builder 对象提供了一系列的方法，可以对 DID 文档的内容做修改和管理。详细参见 [javadoc](https://todo/url/to/javadoc)。

DIDDocument.Builder object provides a series of methods to modify and manage the contents of DID documents. See Javadoc for details.
