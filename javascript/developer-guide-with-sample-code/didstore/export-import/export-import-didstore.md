# Export/import DIDStore

DID Store 提供导出整个DIDStore的方法，导出内容有两个部分：RootIdentity和DID。

DID Store provides a method to export the entire DIDStore. RootIdentity and DID are the exported content.

Export输出结果是zip格式文件。获取DID Store内多个RootIdentity和DID，按前两章节导出方法获取数据文件，保存在zip中。

The exported result is a zip file. To get multiple RootIdentities and DIDs from the DID store, users need to obtain data files according to the export methods in the previous two chapters, and then save them in zip file.

DID Store 提供导入DID Store的方法，直接导入zip文件以RootIdentity和DID对象保存在导入的DID Store，完成整个DID Store的迁移。

DID Store provides the method of importing DID Store by directly importing zip files and saving them as RootIdentity and DID objects in the imported DIDstore, thus completing the migration of the whole DID Store.

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

Password is the export password used for encrypting the private key of DID, which is required when importing data; storepass is the store password for exporting DID store, and users obtain the private key from the DID store.

```typescript
public async importStore(
    zipFile: string,
    password: string,
    storepass: string
): Promise<void>;
```

zipFile为zip文件路径；password为导入密码，和导出密码一致，如果错误就无法正确完成导入功能；storepass为导入DID Store的store password，用于私钥在DID Store的加密。

zipFile is the zip file path; password is the import password, which is consistent with the export password. If it is wrong, the import function cannot be completed correctly. Storepass is the store password for importing DID store, which is used for the encryption of the private key in the DID Store.
