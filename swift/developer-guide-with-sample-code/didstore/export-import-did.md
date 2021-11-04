# Export/import DID

导出指定DID的DIDDocument到给定路径，包括：文档、凭据、私钥及其元数据，导出数据格式为.zip

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

导入指定DID的DIDDocument包括：文档、凭据、私钥及其元数据

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

导出指定DID的RootIdentity包括：助记词、私钥等其元数据，导出数据格式为.zip

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

导入指定DID的RootIdentity包括：助记词、私钥等其元数据

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

导出此DIDStore下的所有DID数据，导出数据格式为.zip

```
let exportFile = "YOUR-EXPORT-PATH"
// "password":用于加密导出数据中的私钥的密码, "storePassword":原存储密码
try store.exportStore(to: exportFile, using: "password", storePassword: "storePassword")
```

导入一个导出的DIDStore到指定的DIDStore 路径：

```
let importPath = "YOUR-IMPORT-ROOT-PATH" // 
//documentPath：需要导入到DIDStore的.zip路径
let documentPath = "YOUR-DOCUMENT-PATH"
let store = try DIDStore.open(atPath: importPath)
// "password": 导入数据的加密密码，"storePassword"：导入数据时存储在本地的加密密码
try store.importStore(from: documentPath, using: "password", storePassword: "storePassword")
```
