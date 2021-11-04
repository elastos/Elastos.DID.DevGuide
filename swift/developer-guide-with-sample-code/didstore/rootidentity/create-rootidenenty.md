---
description: 创建RootIdenenty
---

# Create RootIdenenty

通过给定的Mnemonic创建RootIdentity：

```
let store = try DIDStore.open(atPath: "STORE-ROOT-PATH")
let mnemonic = try Mnemonic.generate(Mnemonic.DID_ENGLISH)
// PASS-PHRASE: the password for mnemonic to generate seed, STORE-PASSWORD: 本地数据存储密码
let identity = try RootIdentity.create(mnemonic, "PASS-PHRASE", true, store, "STORE-PASSWORD")
```

不使用PASS-PHRASE来创建RootIdentity：

```
let store = try DIDStore.open(atPath: "STORE-ROOT-PATH")
let mnemonic = try Mnemonic.generate(Mnemonic.DID_ENGLISH)
let identity = try RootIdentity.create(mnemonic, true, store, "STORE-PASSWORD")
```

从根私钥创建RootIdentity：

```
let store = try DIDStore.open(atPath: "STORE-ROOT-PATH")
let identity = try RootIdentity.create(with: "EXTENTED-PRIVATE-KEY", true, store, "STORE-PASSWORD")
```

通过衍生公钥生成一个只有公钥的RootIdentity：

```
let store = try DIDStore.open(atPath: "STORE-ROOT-PATH")
// preDerivedPublicKey：the pre-derived extended public key，index: current available derive index
let identity = try RootIdentity.create("PRE-DERIVED-PUBLIC-KEY",1)
```

