# Export/import DIDStore

DID store 提供导出整个DIDStore的方法，导出内容有两个部分：RootIdentity和DID。

DID store provides a method to export the whole DID Store. The exported content has two parts: Root Identity and DID.

Export输出结果是zip格式文件。获取DID store内多个RootIdentity和DID，按前两章节导出方法获取数据文件，保存在zip中。

The Export result is a zip file. Get multiple Root Identity and DIDs in DID store, get data files according to the export method in the previous two chapters, and save them in zip.

DID Store 提供导入DID store的方法，直接导入zip文件以RootIdentity和DID对象保存在导入的DID store，完成整个DID store的迁移。

DID Store provides the method of importing DID store, directly importing zip files and saving them in the imported DID store with Root Identity and DID objects, thus completing the migration of the whole DID store.

## Example

```c
//export DID Store
const char *zipfile = "/did/store.zip";
if (DIDStore_ExportStore(store, storePass, zipfile, "pwd") < 0)
		//error operation
  
if (DIDStore_ImportStore(newStore, newStorePass, zipfile, "pwd") < 0)
		//error operation
  
//do something in the new DID Store
... ... ... ...
```

## Usage

```c
int DIDStore_ExportStore(
        DIDStore *store,
        const char *storepass,
        const char *zipfile,
        const char *password);
```

该方法提供整个DID Store的导出方法。`store`为需要导出的DID Store；`storepass`为`store`的store pass；`zipfile`为导出的zip文件路径；`password`为导出密码，用于DID 私钥加密，导入数据时需要使用。

This method provides the export method of the whole DID Store. _store_ is the DID Store to be exported; _storepass_ is the store pass of store; _zipfile_ is the path of the exported zip file; _password_ is the export password, used for DID private key encryption, which is required when importing data.

```c
int DIDStore_ImportStore(
        DIDStore *store,
        const char *storepass,
        const char *zipfile,
        const char *password);
```

该方法提供对整个DID Store的导入方法。`store`为导入的新DID Store；`storepass`为`store`的pass word，用于导入的私钥加密；`zipfile`为导入zip文件路径；`password`为导入密码，和导出密码一致，如果错误就无法正确完成导入功能

This method provides an import method for the whole DID Store. store is the imported new DID Store; _storepass_ is the password of the store, which is used to encrypt the imported private key; _zipfile_ is the path to import zip file; _password_ is the import password, which is the same with the export password. If it is wrong, the import function cannot be completed correctly.
