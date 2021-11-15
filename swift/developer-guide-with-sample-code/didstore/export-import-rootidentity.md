# Export/import RootIdentity

DIDStore 对象提供了对 RootIdentity 导出/导入的方法，可以实现将特定 RootIdentity 相关的助记词、公私钥以及其他相关的数据导出到一个 JSON 文件中。在导出文件中的 PrivateKey 是加密保存的，密钥为导出时设定的导出文件密码。导出的 JSON 是一个没有任何依赖的自相关单一文件，应用可以方便的使用导出文件迁移 RootIdentity 的数据。

## 导出RootIdentity

```
// 例："did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y"
let id = "YOUR-DIDSTRING"
let rootPath = "YOUR-EXPORT-PATH"
let exportName = "didexport.json"
let exportFile = rootPath + exportName
FileManager.default.createFile(atPath: exportFile, contents: nil, attributes: nil)
let fileHndle: FileHandle = FileHandle(forWritingAtPath: exportFile)!
// "password":用于加密导出数据中的私钥的密码, "storePassword":原存储密码
try store!.exportRootIdentity(id, to: fileHndle, using: "password", storePassword: storePassword)
```

和 Store 导出一样，DIDStore import 的输入也可以是 FileHandle、OutputStream 等对象。

## 导入RootIdentity

```
let importPath = "YOUR-IMPORT-ROOT-PATH"
//documentPath：需要导入到DIDStore的.zip路径
let documentPath = "YOUR-DOCUMENT-PATH"
let store = try DIDStore.open(atPath: importPath)
let readerHndle = FileHandle(forReadingAtPath: documentPath)
readerHndle?.seek(toFileOffset: 0)
// "password": 导入数据的加密密码，"storePassword"：导入数据时存储在本地的加密密码
try store.importRootIdentity(from: readerHndle!, using: "password", storePassword: storePassword)
```

和 RootIdentity  导出一样，RootIdentity  import 的输入也可以是 FileHandle、InputStream 等对象。

如果 DIDStore 中已经存在导出文件对应的 RootIdentity，那么 import 操作将覆盖目标 store 中 RootIdentity 现有的数据。

> RootIdentity 也可以通过助记词进行备份和恢复，详细参见 [RootIdentity 的备份和恢复](../rootidentity/backup-restore-rootidentity.md)。

