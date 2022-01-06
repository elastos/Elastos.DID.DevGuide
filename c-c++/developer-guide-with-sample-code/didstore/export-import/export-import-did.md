# Export/import DID

DID store 提供导出指定DID的方法，导出内容主要有DID，DID Document，Verifiable Credentials（如果存在）和private keys（如果存在）。

DID store provides a method to export the specified DID, which mainly includes DID, DID Document, Verifiable Credentials (if any) and private keys (if any).

Export输出结果是json格式字符串，DID的相关导出内容都是以json字段按顺序保存在json里。为了安全起见，其中private key加密保存。

The Export result is a string in json format, and the related export contents of DID are saved in json in the form of json fields successively. For security reasons, the private key is encrypted and saved.

DID store 提供导入DID的方法，直接将json数据以DID对象保存在导入的DID store，完成DID的迁移。

DID store provides the method of importing DID, and directly saves json data in the imported DID store as a DID object to complete the migration of DID.

## Example

```c
//export DID
const char *filepath = "/did/context.json";
if (DIDStore_ExportDID(store, storePass, did, filepath, "pwd") < 0)
  //error operation

if (DIDStore_ImportDID(newStore, newStorePass, filepath, "pwd") < 0)
  //error operation
  
//do something about did in new DID Store
... ... ... ...
```

## Usage

```c
int DIDStore_ExportDID(
        DIDStore *store,
        const char *storepass,
        DID *did,
        const char *file,
        const char *password);
```

`storepass`为导出DID Store的store pass，用户从DID Store获取私钥；`file`是用户提供的用来保存导出结果的文件路径，该文件可以不存在；`password`为导出密码，用于DID 私钥加密，导入数据时需要使用；

_storepass_ is the store pass for exporting DID Store, and the user obtains the private key from DID Store; _file_ is the file path provided by the user to save the export result, which may not exist; _password_ is the export password, which is used for DID private key encryption and needs to be used during the data import.

```c
int DIDStore_ImportDID(
        DIDStore *store,
        const char *storepass,
        const char *file,
        const char *password);
```

`file`是用户提供的用来导入DID内容的文件路径；`password`为导入密码，和导出密码一致，如果错误就无法正确完成导入功能；`storepass`为导入DID store的store pass，用于私钥在DID Store的加密。

_file_ is the file path provided by the user to import DID content; _password_ is the import password, which is the same with the export password. If it is wrong, the import function cannot be completed correctly. _storepass_ is the store pass imported into DID store, which is used to encrypt the private key in DID Store.
