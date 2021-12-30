# Modify and update the DID Document

DIDDocument 是 sealed 对象，不能直接进行修改。DID SDK 提供了一个修改 DID 文档的机制，可以在原有文档的基础上创建一个可修改的副本，并在完成修改的同时，重新对该文档进行签名，并生成一个新的文档实例。DID持有人可以用更新的模式发布这个 DID 文档，从而实现 DIDDocument 的更新。发布过程参见[发布DID](publish-did.md)。

DIDDocument is a sealed object which cannot be directly modified. DID SDK provides a mechanism to modify the DID document. According to this mechanism, we can create a modifiable copy based on the original document, modify and re-sign this document to generate a new document instance. The DID controller can update the DID document by publishing it in the updated mode. See “Publish DID” for the detailed publishing process.

在 Swift SDK 的实现中，编辑 DID 文档是通过 DIDDocumentBuilder 对象来实现的。参考示例如下：

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

自定义 DID 的编辑类似，但是需要 DID 的有效持有人身份才能进行编辑；多签的 DID 编辑完成后需要采用和[创建多签自定义 DID](create-multi-signed-customized-did.md) 中类似的流程对 DIDDocument 进行签名。

The customized DID can be edited in a similar way, but only the valid controller of DID can edit the document; After editing the multi-signed DID, a process similar to that of a multi-signed customized DID should be created and adopted to sign the DIDdocument.

DIDDocumentBuilder 对象提供了一系列的方法，可以对 DID 文档的内容做修改和管理。

DIDDocumentBuilder offers a series of methods to modify and manage the content of DID documents.
