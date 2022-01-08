# Synchronize

DID store提供方法用来一次性更新store中所有的DID对象，获取并保存最合适的DID Document对象。

DID store provides methods to update all DID objects in the store at one time, as well as obtain and save the most suitable DID document object.

该方法不仅会同步DID store里所有的RootIdentity，还会同步保存在DID store里的自定义的DID，实现DID store全DID全同步。

This method synchronizes not only all the RootIdentities in the DID store, but also the customized DIDs stored in the DID store, thereby realizing full synchronization of all the DIDs in the DID store.

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

handle是用户提供的当本地和链上获取的DID Document发生冲突时的解决方案。SDK提供默认的解决方案是保留本地DID Document，忽略链上版本。若用户有不同的解决方案，需要设置该参数。

Handle is a solution provided by users when locally acquired DID document and that acquired from the chain conflict with each other. The default solution provided by SDK is to keep locally acquired DID document and ignore the version obtained from the chain. To apply different solutions, users need to set this parameter.
