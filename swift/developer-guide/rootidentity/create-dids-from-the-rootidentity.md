# Create DIDs from the RootIdentity

在 Elastos DID SDK 中，所有的普通 DID 都是从 RootIdentity 创建，每个创建的 DID 对应一个整数索引值，索引的值空间是 0 \~ 2,147,483,647。每个索引值和其创建的 DID 是唯一的对应关系，即该索引创建的 DID 永远是一致的。

In Elastos DID SDK, all ordinary DIDs are created from RootIdentity, each of which corresponds to an integer index value that ranges from 0 to 2,147,483,647. There is a unique correspondence between each index value and the DID created by it, that is, the index value is always consistent with the DID it creates.

从 RootIdentity 创建 DID 时，新创建的 DID 对应的 DIDDocument 和 PrivateKey 默认会保存到 RootIdentity 所在的 DIDStore 中。新创建的 DID 不会自动进行发布，仅仅是一个本地的 DID 对象，如果需要发布，那么需要显式的进行发布操作，参见[发布 DID](../did/publish-did.md)。

When creating a DID from RootIdentity, DIDdocument and PrivateKey corresponding to the newly created DID will be saved by default to the DIDStore where RootIdentity is located. Instead of being published automatically, the newly created DID is only a local DID object. If it needs to be published, it needs to be published explicitly. See “Publish DID” for details.

## 使用默认索引创建DID

Create DIDs with the default index

```
let store = try DIDStore.open(atPath: "STORE-ROOT-PATH")
let mnemonic = try Mnemonic.generate(Mnemonic.DID_ENGLISH)
let identity = try RootIdentity.create(mnemonic, "PASSPHRASE", true, store, "STORE-PASSWORD")
let did = try identity.getDid(0)
let document = try identity.newDid("STORE-PASSWORD")
```

使用默认索引创建 DID 时，不需要指定索引值，RootIdentity 会使用当前记录的下一个可用索引创建，并且自动更新下一个可用索引。**推荐使用这种方式创建新的 DID**。

To create DIDs with the default index, there is no need to specify the index value. RootIdentity will create DIDs with the next available index of the current record and automatically update the next available index. This method is recommended to create new DIDs.

## 使用指定Index索引创建DID

Create DIDs with the specified index

```
let store = try DIDStore.open(atPath: "STORE-ROOT-PATH")
let mnemonic = try Mnemonic.generate(Mnemonic.DID_ENGLISH)
let identity = try RootIdentity.create(mnemonic, "PASSPHRASE", true, store, "STORE-PASSWORD")
let did = try identity.getDid(0)
let document = try identity.newDid(0, "STORE-PASSWORD")
```

在指定索引创建 DID 时，指定的索引值不被 RootIdentity 管理，所以需要开发者或者应用来管理可用的索引，并避免重复使用索引引起的冲突。

To create DIDs with the specified index, the specified index value is not managed by RootIdentity. Therefore, developers or applications should manage available indexes to avoid conflicts caused by repeated use of indexes.

## 使用RootIdentity创建DID

Create DIDs with RootIdentity

```
let alias = "my first did"
let document = try identity.newDid("STORE-PASSWORD")
document.getMetadata().setAlias(alias)
```

## 从ID链同步DID（同步操作）

Synchronize DIDs from the ID chain (synchronous operation)

```
let storePath = try DIDStore.open(atPath: "YOUR-ROOT-PATH")
let rootIdentity = try RootIdentity.create(mnemonic, "PASSPHRASE", true, storePath, "STORE-PASSWORD")
// 同步方法
try rootIdentity.synchronize()
```

## 从链上同步DID（异步操作）

Synchronize DIDs from the chain (asynchronous operation)

```
let storePath = try DIDStore.open(atPath: "YOUR-ROOT-PATH")
let rootIdentity = try RootIdentity.create(mnemonic, "PASSPHRASE", true, storePath, "STORE-PASSWORD")
// 异步方法
try rootIdentity.synchronizeAsync()
```

## 重新创建 DID 对象和其文档

Re-create DID objects and their files

如果需要重新创建已经创建过的 DIDs，默认情况下会因为 DIDs 已经存在而失败，可以使用具有 overwrite 参数的方法创建，如果 overwrite 为 true 的时候，DID SDK 将忽略已经存在的 DID 对象，并重新创建对应的 DID 文档对象，并重新生成其对应的 PrivateKey。

If you need to re-create the DIDs that have already been created, it will fail by default because of the DIDs that already exist. So, we can re-create the DIDs using the method with overwrite parameters. If the overwriting is true, DID SDK will ignore the already existing DID object, re-create the corresponding DID file, and regenerate its corresponding PrivateKey.

```
let store: DIDStore // an opened DIDStore instance
let storePasswd = "secret"
let identity: RootIdentity = ... // a RootIdentity instance
let index = 8 // the specified index
let overwrite = true

let doc = try identity.newDid(index, overwrite, storePasswd)
```

## 导出助记词

Export mnemonic

```
let myMnemonic = identity.exportMnemonic("STORE-PASSWORD")
```
