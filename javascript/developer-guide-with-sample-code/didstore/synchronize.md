# Synchronize

DID Store provides methods to update all DID objects within at one time, as well as obtain and save the most suitable DID document object.

This method synchronizes not only all the RootIdentities in the DID Store but also customized DIDs stored within, thereby realizing full synchronization of all the DIDs in the DID Store.

## Example

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
... ... ... ...
//Synchronize
await store.synchronize();
//to check DID object or use the newest DID Document
... ... ... ...
store.close();
```

## Usage

```typescript
public async synchronize(
    handle: ConflictHandle = null
): void;
```

Handle is a solution provided by users when locally acquired DID document and that acquired from the chain conflict with each other. The default solution provided by the SDK is to keep the locally acquired DID document and ignore the version obtained from the chain. To apply different solutions, users need to set this parameter.
