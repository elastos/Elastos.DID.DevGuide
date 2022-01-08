# Metadata of the DID objects

为了方便应用开发，DIDStore 为在 DIDStore 存储的 DID 对象提供了保存一些本地数据的能力，DID SDK 把这些数据称为 DID 对象的 metadata。

To better develop the applications, DIDStore provides DID objects stored in DIDStore with the ability to save some local data. DID SDK refers to such data as the metadata of DID objects.

不同的 DID 对象实现，根据自身的需要提供了不同的 metadata 数据。比较典型的是 DIDMetadata 和 CredentialMetadata。DID 和可验证凭证都可以从对象获取其 metadata 接口，并存取相关的数据。例如：

The implementations of different DID objects provide different metadata according to their own needs. DIDMetadata and CredentialMetadata are two typical examples. Both DID and verifiable credentials can get their metadata interfaces from the DID objects and access related data.Example:

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

The interface of Metadata may differ in SDKs implemented in different languages, so it should be used reasonably according to specific SDKs.

另外 RootIdentity 和 DIDStore 也有使用 store 提供的 metadata 机制，只是因为实现的需求，没有直接提供 Metadata 接口，但其对象的本地数据都也会使用到 metadata。

Moreover, RootIdentity and DIDStore also use the metadata mechanism provided by the store. Due to the need of implementation, the metadata interface is not directly provided, but the metadata will also be used by the local data of their objects.
