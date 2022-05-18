# Export/Import RootIdentity

DIDStore objects provide a method to export/import RootIdentity, which can export mnemonic, public, and private keys, as well as other data related to a specific RootIdentity to a JSON file. The PrivateKey is saved encrypted in the exported file, with the key being the password of the exported file set at the time of exporting. The exported JSON is a single autocorrelation file without any dependence, and the application can easily use the exported file to migrate the data of RootIdentity.

## Export RootIdentity

```
// 例："did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y"
let id = "YOUR-DIDSTRING"
let rootPath = "YOUR-EXPORT-PATH"
let exportName = "didexport.json"
let exportFile = rootPath + exportName
FileManager.default.createFile(atPath: exportFile, contents: nil, attributes: nil)
let fileHndle: FileHandle = FileHandle(forWritingAtPath: exportFile)!
// "password":用于加密导出数据中的私钥的密码, "storePassword":原存储密码
try store.exportRootIdentity(id, to: fileHndle, using: "password", storePassword: storePassword)
```

Similar to the export of Store, the DIDStore input can also be objects such as FileHandle and OutputStream.

## Import RootIdentity

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

Like the export of RootIdentity, objects such as FileHandle and InputStream can also be the input of RootIdentity import.

If the RootIdentity corresponding to the exported file already exists in DIDStore, then the import operation will overwrite the existing data of RootIdentity in the target store.

> RootIdentity can also be backed up and restored by mnemonic. For specifics, see [“Backup and Recovery of RootIdentity”.](../rootidentity/backup-restore-rootidentity.md)
