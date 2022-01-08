# Export/Import DID

DIDStore objects provide methods to export DID files, verifiable credentials, private keys, and other data related to a specific DID to a JSON file. The PrivateKey is saved encrypted by the exported file, with the key being the password of the file set at the time of exporting. The exported JSON file is a single autocorrelation file without any dependence and the application can easily use the exported file to migrate the data of DID (the format of the exported data is. zip).

## Export DID

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

## Import DID

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
