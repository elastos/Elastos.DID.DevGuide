# Create DID and DIDDocument

As two different representations of the same object, DID and DIDDocument are created at the same time - ordinary DIDs are created from RootIdentity. See the [RootIdentity document](../../developer-guide-with-sample-code/rootidentity/create-dids-from-the-rootidentity.md) for the method of creating DIDs from RootIdentity.

## From RootIdentity

```
// STORE-PASSWORD用于本地一些数据加密的密码
let document = try identity.newDid("STORE-PASSWORD")
// 获取subject
let subject = document.subject
// 获取contollers
let contollers = document.contollers()
```

## Import DIDDocument

```
// 从路径加载document
let contollers = DIDDocument.convertToDIDDocument(fromFileAtPath: "YOUR-FILE-PATH-STRING")
// 从路径URL获取
let contollers = DIDDocument.convertToDIDDocument(fromUrl: URLPath)
```
