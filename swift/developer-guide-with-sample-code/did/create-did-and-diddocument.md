# Create DID and DIDDocument
DID 和 DIDDocument 是同一个对象的两个不同表现，并且同时被创建。普通 DID 都是通过 RootIdentity 创建，从 RootIdentity 创建 DID的方法参见 [RootIdentity 的文档](../rootidentity/create-dids-from-the-rootidentity.md)。

## 通过RootIdentity

```
// STORE-PASSWORD用于本地一些数据加密的密码
let document = try identity.newDid("STORE-PASSWORD")
// 获取subject
let subject = document.subject
// 获取contollers
let contollers = document.contollers()
```

## 导入DIDDocument

```
// 从路径加载document
let contollers = DIDDocument.convertToDIDDocument(fromFileAtPath: "YOUR-FILE-PATH-STRING")
// 从路径URL获取
let contollers = DIDDocument.convertToDIDDocument(fromUrl: URLPath)
```

