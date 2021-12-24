# Export/import DID

DIDStore 对象提供了对 DID 导出/导入的方法，可以实现将特定 DID 相关的 DID 文档、可验证凭证、私钥以及其他相关的数据导出到一个 JSON 文件中。在导出文件中的 PrivateKey 是加密保存的，密钥为导出时设定的导出文件密码。导出的 JSON 是一个没有任何依赖的自相关单一文件，应用可以方便的使用导出文件迁移 DID 的数据。

DID Store object provides a method to export and import DID, which can export data related to a specific DID, such as DID documents, verifiable credentials, and private keys to a JSON file. The Private Key in the export file is encrypted and saved, and the key is the passphase of the export file set at the time of export. The exported JSON is an autocorrelation single file without any dependence, and the application can easily use the exported file to migrate DID data.

## 导出 DID

Export DID

```java
DIDStore store; // an opened DIDStore instance
DID did = new DID("did:elastos:ibeTptCepbUFE8qjmAUmQGp1eRuNnXZosL");
String storePasswd = "secret";
String exportPasswd = "passwd"
File exportFile = new File("/tmp/export-sample.json");

store.exportDid(did, exportFile, exportPasswd, storePasswd);
```

为了满足不同的应用需求，DID export的输出目标可以是 File、OutputStream、Write 等对象，详细的方法参见 [Java API 文档](https://todo/url/to/javadoc)。

To meet different application requirements, DID export’s output targets can be File, Output Stream, Write and other objects. See Java API documentation for detailed methods.

## 导入 DID

Import DID



```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
String exportPasswd = "passwd"
File exportFile = new File("/tmp/export-sample.json");

store.importDid(exportFile, exportPasswd, storePasswd);
```

和 DID 导出一样，DID import 的输入也可以是 File、InputStream、Reader 等对象，详细的方法参见 [Java API 文档](https://todo/url/to/javadoc)。

As with DID export, the input of DID import can also be File, Input Stream, Reader, and other objects. See Java API documentation for detailed methods.

如果 DIDStore 中已经存在导出文件对应的 DID，那么 import 操作将覆盖目标 store 中 DID 现有的数据。

If the DID corresponding to the exported file already exists in the DID Store, the import operation will overwrite the existing data of DID in the target store.
