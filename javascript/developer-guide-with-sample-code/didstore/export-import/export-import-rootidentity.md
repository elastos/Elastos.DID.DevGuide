# Export/import RootIdentity

DID store 提供导出指定RootIdentity的方法，内容包括RootIdentity的id，mnemonic，private key，derive index，metadta等。

DID store offers the method of exporting the specified RootIdentity, including the id, mnemonic, private key, derive index, metadta etc. of RootIdentity.

Export输出结果是json格式字符串，RootIdentity的相关导出内容都是以json字段形式挨个保存在json里。为了安全起见，其中private key为加密保存。

The exported result is a string in the format of json, and the exported content of RootIdentity is stored in json one by one in the form of json fields. For security reasons, the private key is encrypted.

DID store 提供导入RootIdentity的方法，直接将json数据以RootIdentity对象保存在导入的DID store，完成RootIdentity的迁移。

DID store provides a method of importing RootIdentity, which directly saves json data in the imported DID store as RootIdentity object, thereby accomplishing the migration of RootIdentity.

## Example

```typescript
let rootPath = "root/store";
let restorePath = "root/newStore";
let store = await DIDStore.open(rootPath);
... ... ... ...
let ids = await store.listRootIdentities();
let id = ids[0].getId();
... ... ... ...  
//export RootIdentity
let data = await store.exportRootIdentity(id, "password", "pwd");
store.close();

if (data != null && data !== "") {
    console.log("export root identity {} successfully.", id);
    let newStore = await DIDStore.open(restorePath);
    //import RootIdentity
    await newStore.importRootIdentity(data, "password", "newpwd");
    let rootId = newStore.loadRootIdentity(id);
    if (rootId)
        console.log("import root identity {} successfully.", id);
    else
        console.log("import root identity {} failed.", id);
    ... ... ... ...
    newStore.close();
} else {
    console.log("export root identity {} failed.", id);
}
```

## Usage

```typescript
public exportRootIdentity(
    id: string,
    password: string,
    storepass: string
): string;
```

id为需要导出RootIdentity的标识符；password为导出密码，用于RootIdentity 私钥加密，导入数据时需要使用；storepass为导出DID store的store password，用户从DID store获取私钥。

Id is the identifier that needs to export RootIdentity; password is the export password used for the encryption of RootIdentity private key, which is required when importing data; storepass is the store password for exporting DID store, and users obtain the private key from the DID store.

```typescript
public importRootIdentity(
    data: string,
    password: string,
    storepass: string
): void;
```

data为json格式的数据，即导出数据；password为导入密码，和导出密码一致，如果错误就无法正确完成导入功能；storepass为导入DID store的store password，用于私钥在DID store的加密。无需提供RootIdentity的id标识符信息。

Data refers to data in json format, that is, the exported data; password is the import password, which is consistent with the export password. If it is wrong, the import function cannot be completed correctly. Storepass is the store password imported into the DID store, which is used for the encryption of private key in the DID store. There is no need to provide any information about the id identifier of RootIdentity.
