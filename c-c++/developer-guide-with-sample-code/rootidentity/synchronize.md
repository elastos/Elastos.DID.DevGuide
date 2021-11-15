# Synchronize

RootIdentity提供方法去获取根身份衍生而来的所有链上的DID，并把相应的DID Document保存到DID store中。

这种方法便于用户一次性更新根身份下的多个DID，获取最新的Document，也可以初始化用户的新DID store。

## Example

```c 
const char *rootPath = "root/store";
DIDStore *store = await DIDStore.open(rootPath);
... ... ... ...
const char *mnemonic = "pact reject sick voyage foster fence warm luggage cabbage any subject carbon";
RootIdentity *identity = RootIdentity_Create(mnemonic, "", store, "pwd", true);
if (!identity)
  	//error operation
  
printf("Synchronizing from IDChain...\n");
//synchronize
if (!RootIdentity_Synchronize(identity, NULL))
  	//error operation
  
... ... ... ...
RootIdentity_Destroy(identity);
... ... ... ...
DIDStore_Close(store);
```

## Usage

```c
bool RootIdentity_Synchronize(RootIdentity *rootidentity, DIDDocument_ConflictHandle *handle);
```

`handle`是用户提供的当本地和链上获取的DID Document发生冲突时的解决方案。SDK提供默认的解决方案是保留本地DID Document，忽略链上版本。若用户有不同的解决方案，需要设置该参数。

```c
bool RootIdentity_SynchronizeByIndex(RootIdentity *rootidentity, int index,
        DIDDocument_ConflictHandle *handle);
```

`index`表示根身份衍生的第几个DID；handle含义如上。

