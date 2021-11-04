# Synchronize

DID store提供方法用来一次性更新store中所有的DID对象，获取并保存最合适的DID Document对象。

该方法不仅会同步DID store里所有的RootIdentity，还会同步保存在DID store里的自定义的DID，实现DID store全DID全同步。

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
