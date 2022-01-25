# Create RootIdentity

RootIdentity is the root identity of the user. To use DID, the user needs to create a root identity object first.

## Create RootIdentity Using Mnemonic

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

The newly created RootIdentity object and its corresponding PrivateKey will be saved to the specified DIDStore. If you need to use the RootIdentity object, you can read it from the store using the [method provided by DIDStore](../didstore/access-didstore.md#access-rootidentity).

The mnemonic used by Elastos DID is compatible with that of [Bitcoin](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki#Serialization\_format), both of which comply with BIP39.

Apart from creating RootIdentity by mnemonic, applications can also create RootIdentity by extending the root private key.
