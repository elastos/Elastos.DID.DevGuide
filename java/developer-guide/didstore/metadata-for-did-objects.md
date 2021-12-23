# Metadata of the DID objects

为了方便应用开发，DIDStore 为在 DIDStore 存储的 DID 对象提供了保存一些本地数据的能力，DID SDK 把这些数据称为 DID 对象的 metadata。

For the convenience of application development, DID Store provides the ability to save some local data for DID objects stored in DID Store, which is called the metadata of DID objects by DID SDK.

不同的 DID 对象实现，根据自身的需要提供了不同的 metadata 数据。比较典型的是 DIDMetadata 和 CredentialMetadata。DID 和可验证凭证都可以从对象获取其 metadata 接口，并存取相关的数据。例如：

Different DID object implementations provide different metadata data according to their own needs. Typical examples are DID Metadata and Credential Metadata. Both DID and verifiable credentials can get their metadata interface from the object and access related data. Example:

```java
DIDStore store; // an opened DIDStore instance
DID did = new DID("did:elastos:iU7yMyvoqEqJQgjX71W627whs1MGmEM5j3");
DIDURL vcId = new DIDURL("did:elastos:iU7yMyvoqEqJQgjX71W627whs1MGmEM5j3#profile");

DIDDocument doc = store.load(did);
DIDMetadata dm = doc.getMetadata();
dm.setAlias("Primary DID");
// ...

VerifiableCredential vc = store.loadCredential(vcId);
CredentialMetadata cm = vc.getMetadata();
String alias = cm.getAlias();
// ...
```

Metadata 的接口在不同语言实现的 SDK 中可能存在差异，根据特定的 SDK 实现合理的使用。

The interface of Metadata may be different in SDKs implemented in different languages, so it can be used reasonably according to specific SDKs.

另外 RootIdentity 和 DIDStore 也有使用 store 提供的 metadata 机制，只是因为实现的需求，没有直接提供 Metadata 接口，但其对象的本地数据都也会使用到 metadata。

In addition, Root Identity and DID Store also use the Metadata mechanism provided by the store, only because of the need of implementation. There is no metadata interface provided directly, but the local data of their objects will also use metadata.
