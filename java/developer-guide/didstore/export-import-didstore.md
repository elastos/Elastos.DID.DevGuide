# Export/import DIDStore

DIDStore 对象提供了对整个 store 进行导出/导入的方法，可以实现将 DIDStore 中所有的 RootIdentity、DID 文档、可验证凭证、私钥以及其他相关的数据导出到一个文件中。在导出文件中的 PrivateKey 是加密保存的，密钥为导出时设定的导出文件密码。导出的文件是一个没有任何依赖的自相关单一 ZIP 文件，应用可以方便的使用导出文件迁移或者备份/回复 DIDStore。

DID Store object provides a method to export and import the whole store, which can export all the Root Identity, DID documents, verifiable credentials, private keys, and other related data in DID Store to one file. The Private Key in the export file is encrypted and saved, and the key is the passphase of the export file set at the time of export. The exported file is an autocorrelation single ZIP file without any dependence, and the application can easily use the exported file to migrate or backup/reply to DID Store.

## 导出 Store

Export Store

```java
DIDStore store; // an opened DIDStore instance

String storePasswd = "secret";
String exportPasswd = "passwd"
File exportFile = new File("/tmp/export-sample.zip");

store.exportStore(exportFile, exportPasswd, storePasswd);
```

为了满足不同的应用需求，DIDStore export的输出目标可以是 File、ZipOutputStream 等对象，详细的方法参见 [Java API 文档](https://todo/url/to/javadoc)。

To meet different application requirements, the output target of DID Store export can be File, Zip Output Stream, and other objects. See Java API documentation for detailed methods.

## 导入 Store

Import Store

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
String exportPasswd = "passwd"
File exportFile = new File("/tmp/export-sample.zip");

store.importStore(exportFile, exportPasswd, storePasswd);
```

和 Store 导出一样，DIDStore import 的输入也可以是 File、ZipInputStream 等对象，详细的方法参见 [Java API 文档](https://todo/url/to/javadoc)。

As with Store export, the input of DID Store import can also be File, Zip Input Stream and other objects. See Java API documentation for detailed methods.

如果 DIDStore 中已经存在导出文件对应的对象，那么 import 操作将覆盖目标 store 中现有的对象数据。

If the object corresponding to the exported file already exists in DID Store, the import operation will overwrite the existing object data in the target store.
