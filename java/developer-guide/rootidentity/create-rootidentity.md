# Create RootIdentity

RootIdentity 是用户的根身份，要使用 DID 就需要先创建一个 RootIdentity 对象。

Root Identity is the root identity of a user. To use DID, a Root Identity object needs to be created first.

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";

// First create a set of mnemonic words
Mnemonic mg = Mnemonic.getInstance();
String mnemonic = mg.generate();

// Ask user save the mnemoinc
System.out.println("Please write down your mnemonic:\n  " + mnemonic);

// Create the root identity.
RootIdentity identity = RootIdentity.create(mnemonic, null, store, storePasswd);
```

新创建的 RootIdentity 对象以及其对应的 PrivateKey 都会保存到指定的 DIDStore 中。后续如果需要使用 RootIdentity 对象，可以通过 DIDStore 的提供的方法[从 store 中读取出该对象](../didstore/access-didstore.md#access-rootidentity)。

The newly created Root Identity object and its corresponding Private Key will be saved to the specified DID Store. It can be read from the store through the method provided by DID Store when it needs to be utilized.

Elastos DID 使用的助记词和 Bitcoin 的助记词是兼容的，都符合 [BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki)。

The mnemonics used by Elastos DID are compatible with those of Bitcoin, and both comply with BIP39.

除了通过助记词创建 RootIdentity，应用还可以通过[扩展根私钥](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki#Serialization\_format)来创建 RootIdentity。

In addition to creating Root Identity by mnemonics, applications can also create Root Identity by extending the root private key.
