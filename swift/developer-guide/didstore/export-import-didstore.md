# Export/import DIDStore

DIDStore 对象提供了对整个 store 进行导出/导入的方法，可以实现将 DIDStore 中所有的 RootIdentity、DID 文档、可验证凭证、私钥以及其他相关的数据导出到一个文件中。在导出文件中的 PrivateKey 是加密保存的，密钥为导出时设定的导出文件密码。导出的文件是一个没有任何依赖的自相关单一 ZIP 文件，应用可以方便的使用导出文件迁移或者备份/回复 DIDStore。

DIDStore objects offers a method to export/import the whole store, which can export all the RootIdentity, DID files, verifiable credentials, private keys and other related data in DIDStore to one file. The PrivateKey is saved encrypted in the exported file, with the key being the password of the exported file set at the time of exporting. The exported file is a single autocorrelation ZIP file without any dependence, and the application can easily use the exported file to migrate or back up/respond to DIDStore.

## 导出 DIDStore

Export DIDStore

```
let exportFile = "YOUR-EXPORT-PATH"
// "password":用于加密导出数据中的私钥的密码, "storePassword":原存储密码
try store.exportStore(to: exportFile, using: "password", storePassword: "storePassword")
```

为了满足不同的应用需求，DIDStore export的输出目标可以是 FileHandle、OutputStream 等对象。

To meet the demands of different applications, the output targets of DIDStore export can be FileHandle, OutputStream etc.

## 导入DIDStore

Import DIDStore

```
let importPath = "YOUR-IMPORT-ROOT-PATH" // 
//documentPath：需要导入到DIDStore的.zip路径
let documentPath = "YOUR-DOCUMENT-PATH"
let store = try DIDStore.open(atPath: importPath)
// "password": 导入数据的加密密码，"storePassword"：导入数据时存储在本地的加密密码
try store.importStore(from: documentPath, using: "password", storePassword: "storePassword")
```

和 Store 导出一样，DIDStore import 的输入也可以是 FileHandle、InputStream 等对象。

Like the export of Store, objects such as FileHandle and InputStream can also be the input of DIDStore import.

如果 DIDStore 中已经存在导出文件对应的对象，那么 import 操作将覆盖目标 store 中现有的对象数据。

If the object corresponding to the exported file already exists in DIDStore, then the import operation will overwrite the existing object data in the target store.
