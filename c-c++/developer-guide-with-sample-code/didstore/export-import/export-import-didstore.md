# Export/import DIDStore

DID store 提供导出整个DIDStore的方法，导出内容有两个部分：RootIdentity和DID。

Export输出结果是zip格式文件。获取DID store内多个RootIdentity和DID，按前两章节导出方法获取数据文件，保存在zip中。

DID Store 提供导入DID store的方法，直接导入zip文件以RootIdentity和DID对象保存在导入的DID store，完成整个DID store的迁移。

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
int DIDStore_ExportStore(DIDStore *store, const char *storepass,
        const char *zipfile, const char *password);
```

该方法提供整个DID Store的导出方法。`store`为需要导出的DID Store；`storepass`为`store`的store pass；`zipfile`为导出的zip文件路径；`password`为导出密码，用于DID 私钥加密，导入数据时需要使用。

```c
int DIDStore_ImportStore(DIDStore *store, const char *storepass,
        const char *zipfile, const char *password);
```

该方法提供对整个DID Store的导入方法。`store`为导入的新DID Store；`storepass`为`store`的pass word，用于导入的私钥加密；`zipfile`为导入zip文件路径；`password`为导入密码，和导出密码一致，如果错误就无法正确完成导入功能
