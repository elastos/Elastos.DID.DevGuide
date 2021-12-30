# Backup and Restore the Root Identity

Root Identity is created based on a set of mnemonics. With mnemonics, the Private Key and Public Key of Root Identity can be recovered. Then, the published DID identity is synchronized by the synchronize method of Root Identity, and the corresponding Private Key is created. This method is suitable for creating a paper key backed up by users.

### Export Mnemonic

```java
RootIdentity identity; // a RootIdentity instance
String storePasswd = "secret";

String mnemonic = identity.exportMnemonic(storePasswd);
System.out.println("Write down the mnemonic: " + mnemonic);
```

> The exported mnemonics need to be stored safely by the application and users. Leaking the mnemonic means that the Root Identity corresponding to the mnemonic and the private keys of all DIDs created by its derivatives are leaked.

### Recover Root Identity from Mnemonics

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";

String mnemonic = "pilot similar sentence quarter labor orange antenna crowd left word mirror then"; // the user saved mnemonic
String passphrase = ""; // an optional passphrase

// create the RootIdentity from the mnemonic and store it in the store
identity = RootIdentity.create(mnemonic, passphrase, store, storePasswd);

// Synchronize all existing and published DIDs derived by this RootIdentity
identity.synchronize();
```

Generally, after restoring the creation of Root Identity through mnemonics, the synchronize method of the Root Identity object will be used to synchronize the DIDs that have been created and published by this root identity to the local store for convenience. When synchronizing DID, the default private key corresponding to DID will also be automatically generated.
