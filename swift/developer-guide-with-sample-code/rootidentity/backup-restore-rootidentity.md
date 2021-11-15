# Backup and restore the RootIdentity

RootIdentity 是基于一组助记词创建的，拥有助记词就可以恢复出其对应的 RootIdentity 的私钥和公钥，然后通过 RootIdentity 的 synchronize 方法同步已经发布的 DID 身份，并创建对应的 PrivateKey。这个方法适用于创建用户备份的 paper key。

### 导出助记词

```
var identity: RootIdentity = ... // a RootIdentity instance
let storePasswd = "secret"

let mnemonic = try identity.exportMnemonic(storePasswd)
```

> 导出的助记词需要应用和用户安全的存放，泄露了助记词意味着助记词对应的 RootIdentity，以及其衍生创建的所有 DID 的私钥泄漏。

### 从助记词恢复 RootIdentity

```
let store: DIDStore // an opened DIDStore instance
let storePasswd = "secret"

let mnemonic = "pilot similar sentence quarter labor orange antenna crowd left word mirror then" // the user saved mnemonic
let passphrase = "" // an optional passphrase

// create the RootIdentity from the mnemonic and store it in the store
identity = try RootIdentity.create(mnemonic, passphrase, store, storePasswd);

// Synchronize all existing and published DIDs derived by this RootIdentity
try identity.synchronize()
```

一般情况下，通过助记词在恢复创建 RootIdentity 后，会使用 RootIdentity 对象的 synchronize 方法，将该根身份已经创建并发布的 DIDs 同步到本地 store，以便于使用。同步 DID 的时候，也会自动的生成 DID 对应的默认私钥。