# Create DIDs from RootIdentity

In the Elastos DID SDK, all ordinary DIDs are created from RootIdentity, each of which corresponds to an integer index value that ranges from 0 to 2,147,483,647. There is a unique correspondence between each index value and the DID created - that is, the index value is always consistent with the DID it creates.

When creating a DID from RootIdentity, DIDdocument and PrivateKey corresponding to it will be saved by default to the DIDStore where RootIdentity is located. Instead of being published automatically, the newly created DID is only a local DID object. If it needs to be published, it needs to be done so explicitly - see [Publish DID](../did/publish-did.md) for details.

## Create DIDs with the Default Index

```
let store = try DIDStore.open(atPath: "STORE-ROOT-PATH")
let mnemonic = try Mnemonic.generate(Mnemonic.DID_ENGLISH)
let identity = try RootIdentity.create(mnemonic, "PASSPHRASE", true, store, "STORE-PASSWORD")
let did = try identity.getDid(0)
let document = try identity.newDid("STORE-PASSWORD")
```

To create DIDs with the default index, there's no need to specify the index value. RootIdentity will create DIDs with the next available index of the current record and automatically update the next available index **(this method is recommended to create new DIDs)**.

## Create DIDs with the Specified Index

```
let store = try DIDStore.open(atPath: "STORE-ROOT-PATH")
let mnemonic = try Mnemonic.generate(Mnemonic.DID_ENGLISH)
let identity = try RootIdentity.create(mnemonic, "PASSPHRASE", true, store, "STORE-PASSWORD")
let did = try identity.getDid(0)
let document = try identity.newDid(0, "STORE-PASSWORD")
```

To create DIDs with the specified index, the specified index value is not managed by RootIdentity. Therefore, developers or applications should manage available indexes to avoid conflicts caused by repeated use of indexes.

## Create DIDs with RootIdentity

```
let alias = "my first did"
let document = try identity.newDid("STORE-PASSWORD")
document.getMetadata().setAlias(alias)
```

## Synchronize DIDs from the IDchain (Synchronous Operation)

```
let storePath = try DIDStore.open(atPath: "YOUR-ROOT-PATH")
let rootIdentity = try RootIdentity.create(mnemonic, "PASSPHRASE", true, storePath, "STORE-PASSWORD")
// 同步方法
try rootIdentity.synchronize()
```

## Synchronize DIDs from the Chain (Asynchronous Operation)

```
let storePath = try DIDStore.open(atPath: "YOUR-ROOT-PATH")
let rootIdentity = try RootIdentity.create(mnemonic, "PASSPHRASE", true, storePath, "STORE-PASSWORD")
// 异步方法
try rootIdentity.synchronizeAsync()
```

## Recreate DID Objects and their Files

If you need to recreate DIDs that have already been developed, this process will fail by default because of the DIDs that already exist. So, we can recreate the DIDs using the method with overwrite parameters. If the overwriting is true, the DID SDK will ignore the already existing DID object, recreate the corresponding DID file, and regenerate its corresponding PrivateKey.

```
let store: DIDStore // an opened DIDStore instance
let storePasswd = "secret"
let identity: RootIdentity = ... // a RootIdentity instance
let index = 8 // the specified index
let overwrite = true

let doc = try identity.newDid(index, overwrite, storePasswd)
```

## Export Mnemonic

```
let myMnemonic = identity.exportMnemonic("STORE-PASSWORD")
```
