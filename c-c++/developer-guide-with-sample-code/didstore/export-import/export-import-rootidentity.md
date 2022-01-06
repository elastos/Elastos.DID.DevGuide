# Export/import RootIdentity

DID Store 提供导出指定RootIdentity的方法，内容包括RootIdentity的id，mnemonic，private key，derive index，metadta等。

DID Store provides a method to export the specified Root Identity, including the ID of Root Identity, mnemonic, private key, derive index, metadata, etc.

Export输出结果是json格式字符串，RootIdentity的相关导出内容都是以json字段形式挨个保存在json里。为了安全起见，其中private key为加密保存。

The Export result is a string in json format, and the related export contents of Root Identity are stored in json in the form of json fields successively. For security reasons, the private key is encrypted and saved.

DID Store 提供导入RootIdentity的方法，直接将json数据以RootIdentity对象保存在导入的DID Store，完成RootIdentity的迁移。

DID Store provides a method of importing Root Identity, which directly saves json data in the imported DID Store as Root Identity object and completes the migration of Root Identity.

## Example

```c
//export RootIdentity
const char *filepath = "/did/default-rootidentity.json";
if (DIDStore_ExportRootIdentity(store, storePass, id, filepath, "pwd") < 0)
  //error operation

if (DIDStore_ImportRootIdentity(newStore, newStorePass, filepath, "pwd") < 0)
  //error operation
  
//do something about RootIdentity in new DID Store
... ... ... ...
```

## Usage

```c
int DIDStore_ExportRootIdentity(
        DIDStore *store,
        const char *storepass,
        const char *id,
        const char *file,
        const char *password);
```

id为需要导出RootIdentity的标识符；password为导出密码，用于RootIdentity 私钥加密，导入数据时需要使用；storepass为导出DID store的store password，用户从DID store获取私钥。

_id_ is the identifier that needs to export Root Identity; _password_ is the export password, used for Root Identity private key encryption, which is required when importing data; _storepass_ is the store password for exporting DID store, and the user gets the private key from DID store.

```c
int DIDStore_ImportRootIdentity(
        DIDStore *store,
        const char *storepass,
        const char *file,
        const char *password);
```

data为json格式的数据，即导出数据；password为导入密码，和导出密码一致，如果错误就无法正确完成导入功能；storepass为导入DID store的store password，用于私钥在DID store的加密。无需提供RootIdentity的id标识符信息。

_data_ is the data in json format, that is, export data; _password_ is the import password, which is the same with the export password. If it is wrong, the import function cannot be completed correctly. _storepass_ is the store password imported into DID store, which is used for encryption of private key in DID store. There is no need to provide id identifier information of Root Identity.
