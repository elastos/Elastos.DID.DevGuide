# Export/import DIDStore

DID store 提供导出整个DIDStore的方法，导出内容有两个部分：RootIdentity和DID。

Export输出结果是zip格式文件。获取DID store内多个RootIdentity和DID，按前两章节导出方法获取数据文件，保存在zip中。

DID store 提供导入DID store的方法，直接导入zip文件以RootIdentity和DID对象保存在导入的DID store，完成整个DID store的迁移。

## Example

```typescript
let rootPath = "root/store";
let restorePath = "root/newStore";
let store = await DIDStore.open(rootPath);
... ... ... ... 
//export DID store
let tempDir = new File(TestConfig.tempDir);
tempDir.createDirectory();
let exportFile = new File(tempDir, "storeexport.zip");

await store.exportStore(exportFile.getAbsolutePath(), "password", "pwd");
store.close();

let newStore = await DIDStore.open(restorePath);
//import DID store
await newStore.importStore(exportFile.getAbsolutePath(), "password", "newpwd");
newStore.close();
```

## Usage

```typescript
public async exportStore(
    zipFile : string,
    password: string,
    storepass: string
): Promise<void>;
```

password为导出密码，用于DID 私钥加密，导入数据时需要使用；storepass为导出DID store的store password，用户从DID store获取私钥。

```typescript
public async importStore(
    zipFile: string,
    password: string,
    storepass: string
): Promise<void>;
```

zipFile为zip文件路径；password为导入密码，和导出密码一致，如果错误就无法正确完成导入功能；storepass为导入DID store的store password，用于私钥在DID store的加密。
