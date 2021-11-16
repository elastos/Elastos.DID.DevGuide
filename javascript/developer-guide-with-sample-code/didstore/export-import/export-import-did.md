# Export/import DID

DID Store 提供导出指定DID的方法，导出内容主要有DID，DID Document，Verifiable Credentials（如果存在）和private keys（如果存在）。

Export输出结果是json格式字符串，DID的相关导出内容都是以json字段按顺序保存在json里。为了安全起见，其中private key加密保存。

DID Store 提供导入DID的方法，直接将json数据以DID对象保存在导入的DID Store，完成DID的迁移。

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

```typescript
public importDid(
    data: string,
    password: string,
    storepass: string
): void;
```

data为json格式的数据，即导出数据；password为导入密码，和导出密码一致，如果错误就无法正确完成导入功能；storepass为导入DID store的store password，用于私钥在DID Store的加密。
