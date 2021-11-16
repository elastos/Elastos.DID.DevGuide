# Local metadata for the DID objects in the store

为了方便应用开发，DIDStore 为在 DIDStore 存储的 DID 对象提供了保存一些本地数据的能力，DID SDK 把这些数据称为 DID 对象的 metadata。

不同的 DID 对象实现，根据自身的需要提供了不同的 metadata 数据。比较典型的是 DIDMetadata 和 CredentialMetadata。DID 和可验证凭证都可以从对象获取其 metadata 接口，并存取相关的数据。例如：

```
let store: DIDStore = ... // an opened DIDStore instance
let did = try DID("did:elastos:iU7yMyvoqEqJQgjX71W627whs1MGmEM5j3")
let vcId = try DIDURL("did:elastos:iU7yMyvoqEqJQgjX71W627whs1MGmEM5j3#profile")

let doc = try store.load(did)
let dm = doc.getMetadata()
dm.setAlias("Primary DID")
// ...

let vc = store.loadCredential(byId: vcId)
let cm = vc.metadata
String alias = cm.getAlias()
// ...
```

Metadata 的接口在不同语言实现的 SDK 中可能存在差异，根据特定的 SDK 实现合理的使用。

另外 RootIdentity 和 DIDStore 也有使用 store 提供的 metadata 机制，只是因为实现的需求，没有直接提供 Metadata 接口，但其对象的本地数据都也会使用到 metadata。