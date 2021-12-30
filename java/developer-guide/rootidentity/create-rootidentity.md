# Create Root Identity

To use DID, a Root Identity object needs to be created first.

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

The newly created Root Identity object and its corresponding Private Key will be saved to the specified DID Store. It can be read from the store through the method provided by [DID Store](../didstore/access-didstore.md#access-rootidentity) when it needs to be utilized.

The mnemonics used by the Elastos DID are compatible with those of Bitcoin, and both comply with [BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki).

In addition to creating Root Identity by mnemonics, applications can also create Root Identity by extending the root private key.
