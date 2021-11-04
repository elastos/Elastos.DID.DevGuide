# Synchronize

RootIdentity提供方法去获取根身份衍生而来的所有链上的DID，并把相应的DID Document保存到DID store中。

这种方法便于用户一次性更新根身份下的多个DID，获取最新的Document，也可以初始化用户的新DID store。

## Example

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

handle是用户提供的当本地和链上获取的DID Document发生冲突时的解决方案。SDK提供默认的解决方案是保留本地DID Document，忽略链上版本。若用户有不同的解决方案，需要设置该参数。

```typescript
public async synchronizeIndex(
    index: number,
    handle: ConflictHandle = null
): Promise<boolean>;
```

index表示根身份衍生的第几个DID；handle含义如上。
