# Backup and synchronize

RootIdentity提供方法去获取根身份衍生而来的所有链上的DID，并把相应的DID Document保存到DID store中。

Rootidentity provides a method to get DIDs on all chains derived from RootIdentity, and save the corresponding DID documents to the DID store.

这种方法便于用户一次性更新根身份下的多个DID，获取最新的Document，也可以初始化用户的新DID store。

This method enables the user to update multiple DIDs under the root identity at one time, get the latest document, and initialize the user’s new DID store.

RootIdentity若是基于mnemonic生成的，那么我们就可以导出该mnemonic进行备份。注意：用扩展根私钥创建的根身份，无法导出助记词。

If the RootIdentity is generated based on mnemonic, then we can export this mnemonic for backup. Note: The root identity created with the extended root private key cannot export mnemonics.

## 导出助记词

Export mnemonic

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

### 恢复RootIdentity

Restore RootIdentity

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

Handle is a solution provided by users when locally acquired DID document and that acquired from the chain conflict with each other. The default solution provided by SDK is to keep locally acquired DID document and ignore the version obtained from the chain. To apply different solutions, users need to set this parameter.

```typescript
public async synchronizeIndex(
    index: number,
    handle: ConflictHandle = null
): Promise<boolean>;
```

index表示根身份衍生的第几个DID；handle含义如上。

Index indicates the n-th DID derived from the root identity; the meaning of handle is the same as above.
