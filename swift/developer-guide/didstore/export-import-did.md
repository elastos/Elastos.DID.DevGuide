# Export/import DID

DIDStore 对象提供了对 DID 导出/导入的方法，可以实现将特定 DID 相关的 DID 文档、可验证凭证、私钥以及其他相关的数据导出到一个 JSON 文件中。在导出文件中的 PrivateKey 是加密保存的，密钥为导出时设定的导出文件密码。导出的 JSON 是一个没有任何依赖的自相关单一文件，应用可以方便的使用导出文件迁移 DID 的数据。导出数据格式为.zip.

Providing a method to export/import DID, DIDStore objects can export DID files, verifiable credentials, private keys and other data related to a specific DID to a JSON file. The PrivateKey in is saved encrypted the exported file, with the key being the password of the exported file set at the time of exporting. The exported JSON file is a single autocorrelation file without any dependence, and the application can easily use the exported file to migrate the data of DID. The format of the exported data is. zip.

## 导出DID

Export DID

```
// 例："did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y"
let did = try DID("YOUR-DIDSTRING")
let rootPath = "YOUR-EXPORT-PATH"
let exportName = "didexport.json"
let exportFile = rootPath + exportName
FileManager.default.createFile(atPath: exportFile, contents: nil, attributes: nil)
let fileHndle = FileHandle(forWritingAtPath: exportFile)
// "password":用于加密导出数据中的私钥的密码, "storePassword":原存储密码
try store.exportDid(did, to: fileHndle, using: "password", storePassword: "storePassword")
```

## 导入DID

Import DID

```
let importPath = "YOUR-IMPORT-ROOT-PATH"
//documentPath：需要导入到DIDStore的.zip路径
let documentPath = "YOUR-DOCUMENT-PATH"
let store = try DIDStore.open(atPath: importPath)
let readerHndle = FileHandle(forReadingAtPath: documentPath)
readerHndle!.seek(toFileOffset: 0)
// "password": 导入数据的加密密码，"storePassword"：导入数据时存储在本地的加密密码
try store.importDid(from: readerHndle!, using: "password", storePassword: "storePassword")
```
