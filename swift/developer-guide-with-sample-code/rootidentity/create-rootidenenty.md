# Create RootIdenenty
RootIdentity 是用户的根身份，要使用 DID 就需要先创建一个 RootIdentity 对象。


## 使用Mnemonic创建

```
let store = try DIDStore.open(atPath: "STORE-ROOT-PATH")
// First create a set of mnemonic words
let mnemonic = try Mnemonic.generate(Mnemonic.DID_ENGLISH)

// Ask user save the mnemoinc
print("Please write down your mnemonic: \(mnemonic)")

// Create the root identity.
// PASS-PHRASE: the password for mnemonic to generate seed, STORE-PASSWORD: 本地数据存储密码
let identity = try RootIdentity.create(mnemonic, "PASS-PHRASE", true, store, "STORE-PASSWORD")
```

新创建的 RootIdentity 对象以及其对应的 PrivateKey 都会保存到指定的 DIDStore 中。后续如果需要使用 RootIdentity 对象，可以通过 DIDStore 的提供的方法[从 store 中读取出该对象](../didstore/access-didstore.md#access-rootidentity)。

Elastos DID 使用的助记词和 Bitcoin 的助记词是兼容的，都符合 [BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki)。

除了通过助记词创建 RootIdentity，应用还可以通过[扩展根私钥](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki#Serialization_format)来创建 RootIdentity。

