# Export/import DID

DID Store 提供导出指定DID的方法，导出内容主要有DID，DID Document，Verifiable Credentials（如果存在）和private keys（如果存在）。

DID Store provides a method to export the specified DID, with the exported content mainly including DID, DID document, verifiable credentials (if any) and private keys (if any).

Export输出结果是json格式字符串，DID的相关导出内容都是以json字段按顺序保存在json里。为了安全起见，其中private key加密保存。

The exported result is a string in json format, and the exported content of DID is orderly stored in json by json field. For security reasons, the private key is stored encrypted.

DID Store 提供导入DID的方法，直接将json数据以DID对象保存在导入的DID Store，完成DID的迁移。

DID Store provides the method of importing DID by directly saving json data as a DID object in the imported DIDstore, thereby completing the migration of DID.

## Example

```typescript
let rootPath = "root/store";
let restorePath = "root/newStore";
let store = await DIDStore.open(rootPath);
... ... ... ...
let dids = await store.listDids();
let did = dids[0];
... ... ... ...  
//export DID
let data = await store.exportDid(did, "password", "pwd");
store.close();

if (data != null && data !== "") {
    console.log("export DID {} successfully.", did);
    let newStore = await DIDStore.open(restorePath);
    //import DID
    await newStore.importDid(data, "password", "newpwd");
    let doc = newStore.loadDid(did);
    if (doc)
        console.log("import DID {} successfully.", did);
    else
        console.log("import DID {} failed.", did);
    newStore.close();
} else {
    console.log("export DID {} failed.", did);
}
```

## Usage

```typescript
public async exportDid(
    did: DID | string,
    password: string,
    storepass: string
): Promise<string>;
```

password为导出密码，用于DID 私钥加密，导入数据时需要使用；storepass为导出DID Store的store password，用户从DID Store获取私钥。

Password refers to the export password used for the encryption of DID private key, which needs to be used when importing data; storepass is the store password for exporting DID Store, and users get the private key from DID Store.

```typescript
public importDid(
    data: string,
    password: string,
    storepass: string
): void;
```

data为json格式的数据，即导出数据；password为导入密码，和导出密码一致，如果错误就无法正确完成导入功能；storepass为导入DID store的store password，用于私钥在DID Store的加密。

Data refers to data in json format, that is, the exported data; password is the import password, which is consistent with the export password. If it is wrong, the import function cannot be completed correctly. Storepass is the store password imported into the DID store, which is used for the encryption of private key in the DID store.
