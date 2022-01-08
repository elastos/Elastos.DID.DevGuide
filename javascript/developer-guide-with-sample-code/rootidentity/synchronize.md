# Backup and Synchronize

RootIdentity provides a method to get DIDs on all chains derived from it, then saveingthe corresponding DID documents to the DID Store. This method enables the user to update multiple DIDs under the root identity at one time, get the latest document, and initialize the user’s new DID Store.

If the RootIdentity is generated based on mnemonic, then we can export this mnemonic for backup. **Note:** The root identity created with the extended root private key cannot export mnemonics.

## Expoert Mnemonic

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
... ... ... ...
let mnemonic = "pact reject sick voyage foster fence warm luggage cabbage any subject carbon";
let rootIdentity = RootIdentity.createFromMnemonic(mnemonic,
    "", store, "pwd", true);
let exportedMnemonic = rootIdentity.exportMnemonic("pwd");
... ... ... ...
store.close();
```

### Restore RootIdentity

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
... ... ... ...
let mnemonic = "pact reject sick voyage foster fence warm luggage cabbage any subject carbon";
let rootIdentity = RootIdentity.createFromMnemonic(mnemonic,
    "", store, "pwd", true);
console.log("Synchronizing from IDChain...");
//synchronize
await rootIdentity.synchronize();
let duration = (Date.now() - start + 500) / 1000;
console.log("Synchronize from IDChain...OK({}s)", duration);
... ... ... ...
//to check DID object
... ... ... ...
store.close();
```

## Usage

```typescript
public async synchronize(
    handle: ConflictHandle = null
): Promise<void>；
```

Handle is a solution provided by users when locally acquired DID document and that acquired from the chain conflict with each other. The default solution provided by the SDK is to keep locally acquired DID document and ignore the version obtained from the chain. To apply different solutions, users need to set this parameter.

```typescript
public async synchronizeIndex(
    index: number,
    handle: ConflictHandle = null
): Promise<boolean>;
```

Index indicates the nth DID derived from the root identity; the meaning of handle is the same as above.
