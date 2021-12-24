# Backup and restore the RootIdentity

RootIdentity 是基于一组助记词创建的，拥有助记词就可以恢复出其对应的 RootIdentity 的私钥和公钥，然后通过 RootIdentity 的 synchronize 方法同步已经发布的 DID 身份，并创建对应的 PrivateKey。这个方法适用于创建用户备份的 paper key。

Root Identity is created based on a set of mnemonics. With mnemonics, the Private Key and public key of Root Identity can be recovered. Then, the published DID identity is synchronized by the synchronize method of Root Identity, and the corresponding Private Key is created. This method is suitable for creating paper key backed up by users.

### 导出助记词

Export mnemonic

```java
RootIdentity identity; // a RootIdentity instance
String storePasswd = "secret";

String mnemonic = identity.exportMnemonic(storePasswd);
System.out.println("Write down the mnemonic: " + mnemonic);
```

> 导出的助记词需要应用和用户安全的存放，泄露了助记词意味着助记词对应的 RootIdentity，以及其衍生创建的所有 DID 的私钥泄漏。
>
> The exported mnemonics need to be stored safely by the application and users. Leaking the mnemonic means that the Root Identity corresponding to the mnemonic and the private keys of all DIDs created by its derivatives are leaked.

### 从助记词恢复 RootIdentity

Recover Root Identity from mnemonics

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

一般情况下，通过助记词在恢复创建 RootIdentity 后，会使用 RootIdentity 对象的 synchronize 方法，将该根身份已经创建并发布的 DIDs 同步到本地 store，以便于使用。同步 DID 的时候，也会自动的生成 DID 对应的默认私钥。

Generally, after restoring the creation of Root Identity through mnemonics, the synchronize method of the Root Identity object will be used to synchronize the DIDs that have been created and published by this root identity to the local store for convenience. When synchronizing DID, the default private key corresponding to DID will also be automatically generated.
