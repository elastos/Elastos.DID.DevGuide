# Export/import RootIdentity

DID store 提供导出指定RootIdentity的方法，内容包括RootIdentity的id，mnemonic，private key，derive index，metadta等。

Export输出结果是json格式字符串，RootIdentity的相关导出内容都是以json字段形式挨个保存在json里。为了安全起见，其中private key为加密保存。

DID store 提供导入RootIdentity的方法，直接将json数据以RootIdentity对象保存在导入的DID store，完成RootIdentity的迁移。

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

```typescript
public importRootIdentity(
    data: string,
    password: string,
    storepass: string
): void;
```

data为json格式的数据，即导出数据；password为导入密码，和导出密码一致，如果错误就无法正确完成导入功能；storepass为导入DID store的store password，用于私钥在DID store的加密。无需提供RootIdentity的id标识符信息。
