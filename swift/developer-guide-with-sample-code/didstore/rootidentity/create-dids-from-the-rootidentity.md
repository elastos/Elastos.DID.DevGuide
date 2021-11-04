# Create DIDs from the RootIdentity

使用RootIdentity创建DIDDocument

```
let alias = "my first did"
let document = try identity.newDid("STORE-PASSWORD")
document.getMetadata().setAlias(alias)
```

通过Index创建DIDDocument：

```
let store = try DIDStore.open(atPath: "STORE-ROOT-PATH")
let mnemonic = try Mnemonic.generate(Mnemonic.DID_ENGLISH)
let identity = try RootIdentity.create(mnemonic, "PASSPHRASE", true, store, "STORE-PASSWORD")
let did = try identity.getDid(0)
let document = try identity.newDid(0, "STORE-PASSWORD")
```

获取DID：通过给定的index，获取DID实例

```
let did = try identity.getDid(1)
```

从ID链同步DID:  同步链上DID到本地有同步方法和异步方法，可根据具体需求选择使用2个方法。

```
let storePath = try DIDStore.open(atPath: "YOUR-ROOT-PATH")
let rootIdentity = try RootIdentity.create(mnemonic, "PASSPHRASE", true, storePath, "STORE-PASSWORD")
// 同步方法
try rootIdentity.synchronize()
```

异步从链上同步DID

```
let storePath = try DIDStore.open(atPath: "YOUR-ROOT-PATH")
let rootIdentity = try RootIdentity.create(mnemonic, "PASSPHRASE", true, storePath, "STORE-PASSWORD")
// 异步方法
try rootIdentity.synchronizeAsync()
```

导出RootIdentity的助记词：

```
let myMnemonic = identity.exportMnemonic("STORE-PASSWORD")
```



