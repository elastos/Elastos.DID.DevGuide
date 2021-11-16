# Export/import RootIdentity

DID Store 提供导出指定RootIdentity的方法，内容包括RootIdentity的id，mnemonic，private key，derive index，metadta等。

Export输出结果是json格式字符串，RootIdentity的相关导出内容都是以json字段形式挨个保存在json里。为了安全起见，其中private key为加密保存。

DID Store 提供导入RootIdentity的方法，直接将json数据以RootIdentity对象保存在导入的DID Store，完成RootIdentity的迁移。

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

```c
int DIDStore_ImportRootIdentity(
        DIDStore *store,
        const char *storepass,
        const char *file,
        const char *password);
```

data为json格式的数据，即导出数据；password为导入密码，和导出密码一致，如果错误就无法正确完成导入功能；storepass为导入DID store的store password，用于私钥在DID store的加密。无需提供RootIdentity的id标识符信息。
