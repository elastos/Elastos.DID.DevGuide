# Backup and Restore RootIdentity

RootIdentity is created based on a set of mnemonics. With mnemonics, the private and public keys of RootIdentity can be recovered. Then, the published DID identity is synchronized by RootIdentity, and the corresponding private key is created. This method is applicable to creating paper keys backed up by users.

### Export Mnemonic

```
var identity: RootIdentity = ... // a RootIdentity instance
let storePasswd = "secret"
let mnemonic = try identity.exportMnemonic(storePasswd)
```

> Applications and users should safely store the exported mnemonic. If the mnemonic is exposed, the RootIdentity corresponding to the mnemonic and the private keys of all DIDs created by its derivatives will be leaked.

### Restore RootIdentity from Mnemonic

Restore the RootIdentity from mnemonic

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

Generally, after restoring the creation of RootIdentity through mnemonics, the synchronization method of the RootIdentity object will be used to synchronize the DIDs that have been created and published by this root identity to the local store for convenience. When synchronizing DIDs, the default private keys corresponding to these DIDs will be automatically generated.
