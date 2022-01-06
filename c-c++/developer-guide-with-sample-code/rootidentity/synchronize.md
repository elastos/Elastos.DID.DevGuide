# Backup and synchronize

RootIdentity提供方法去获取根身份衍生而来的所有链上的DID，并把相应的DID Document保存到DID store中。

Root Identity provides a method to get DIDs on all chains derived from Root Identity and saves the corresponding DID Documents in DID store.

这种方法便于用户一次性更新根身份下的多个DID，获取最新的Document，也可以初始化用户的新DID store。

This method is convenient for the user to update multiple DIDs under the root identity at one time, get the latest Document, and initialize the user’s new DID store.

### 导出助记词

Derive mnemonic

```c
const char *rootPath = "root/store";
DIDStore *store = await DIDStore.open(rootPath);
... ... ... ...
const char *mnemonic = "pact reject sick voyage foster fence warm luggage cabbage any subject carbon";
RootIdentity *identity = RootIdentity_Create(mnemonic, "", store, "pwd", true);
const char *id = RootIdentity_GetId(identity);

char exportMnemonic[ELA_MAX_MNEMONIC_LEN + 1];
if (DIDStore_ExportRootIdentityMnemonic(store, "pwd",
            id, exportMnemonic, sizeof(exportMnemonic)) < 0)
   //error operation
   
 RootIdentity_Destroy(identity);
 ... ... ... ...
 DIDStore_Close(store);
```

### 恢复RootIdentity

Restore Root Identity

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
bool RootIdentity_Synchronize(
    RootIdentity *rootidentity,
    DIDDocument_ConflictHandle *handle);
```

`handle`是用户提供的当本地和链上获取的DID Document发生冲突时的解决方案。SDK提供默认的解决方案是保留本地DID Document，忽略链上版本。若用户有不同的解决方案，需要设置该参数。

_handle_ is a solution provided by users when local and chain-acquired DID Document conflict. The default solution provided by SDK is to keep local DID Document and ignore the version of the chain. If users have different solutions, this parameter needs to be set.

```c
bool RootIdentity_SynchronizeByIndex(
        RootIdentity *rootidentity,
        int index,
        DIDDocument_ConflictHandle *handle);
```

`index`表示根身份衍生的第几个DID；handle含义如上。

_index_ indicates which DID is derived from the root identity; The meaning of handle is as above.
