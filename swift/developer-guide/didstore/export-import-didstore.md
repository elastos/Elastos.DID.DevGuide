# Export/Import DIDStore

DIDStore objects offers a method to export/import the whole store, which can export all the RootIdentity, DID files, verifiable credentials, private keys, and other related data in DIDStore to one file. The PrivateKey is saved encrypted in the exported file, with the key being the password of the exported file set at the time of exporting. The exported file is a single autocorrelation .zip file without any dependence, and the application can easily use the exported file to migrate or back up/respond to DIDStore.

## Export DIDStore

```
let exportFile = "YOUR-EXPORT-PATH"
// "password":用于加密导出数据中的私钥的密码, "storePassword":原存储密码
try store.exportStore(to: exportFile, using: "password", storePassword: "storePassword")
```

To meet the demands of different applications, the output targets of DIDStore export can be FileHandle, OutputStream etc.

## Import DIDStore

```
let importPath = "YOUR-IMPORT-ROOT-PATH" // 
//documentPath：需要导入到DIDStore的.zip路径
let documentPath = "YOUR-DOCUMENT-PATH"
let store = try DIDStore.open(atPath: importPath)
// "password": 导入数据的加密密码，"storePassword"：导入数据时存储在本地的加密密码
try store.importStore(from: documentPath, using: "password", storePassword: "storePassword")
```

Similarly to the export of Store, objects such as FileHandle and InputStream can also be the input of DIDStore import.

If the object corresponding to the exported file already exists in DIDStore, then the import operation will overwrite the existing object data in the target store.
