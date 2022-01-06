# Synchronize

DID Store提供方法用来一次性更新store中所有的DID对象，获取并保存最合适的DID Document对象。

DID Store provides a method to update all DID objects in the store at one time, as well as obtain and save the most suitable DID Document object.

该方法不仅会同步DID Store里所有的RootIdentity，还会同步保存在DID Store里的自定义的DID，实现DID Store全DID全同步。

This method will not only synchronize all the Root Identity in the DID Store, but also synchronize the customized DID saved in the DID store, so as to realize full DID synchronization in the DID Store.

## Example

```c
const char *rootPath = "root/store";
DIDStore *store = DIDStore_Open(rootPath);
... ... ... ...
//Synchronize
DIDStore_Synchronize(store, NULL);
//to check DID object or use the newest DID Document
... ... ... ...
DIDStore_Close(store);
```

## Usage

```c
void DIDStore_Synchronize(
    DIDStore *store,
    DIDDocument_ConflictHandle *handle);
```

`handle`是用户提供的当本地和链上获取的DID Document发生冲突时的解决方案。SDK提供默认的解决方案是保留本地DID Document，忽略链上版本。若用户有不同的解决方案，需要设置该参数。

_handle_ is a solution provided by users when local and chain-acquired DID Document conflict. The default solution provided by SDK is to keep local DID Document and ignore the version of the chain. If users have different solutions, this parameter needs to be set.
