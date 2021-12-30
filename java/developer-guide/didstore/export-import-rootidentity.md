# Export/import RootIdentity

DIDStore 对象提供了对 RootIdentity 导出/导入的方法，可以实现将特定 RootIdentity 相关的助记词、公私钥以及其他相关的数据导出到一个 JSON 文件中。在导出文件中的 PrivateKey 是加密保存的，密钥为导出时设定的导出文件密码。导出的 JSON 是一个没有任何依赖的自相关单一文件，应用可以方便的使用导出文件迁移 RootIdentity 的数据。

DID Store object provides a method to export and import Root Identity, which can export mnemonics, public and private keys and other data related to a specific Root Identity to a JSON file. The Private Key in the export file is encrypted and saved, and the key is the passphase of the export file set at the time of export. The exported JSON is an autocorrelation single file without any dependence, and the application can easily use the exported file to migrate the data of Root Identity.

## 导出 RootIdentity

Export Root Identity

```java
DIDStore store; // an opened DIDStore instance
String id = "d2f3c0f07eda4e5130cbdc59962426b1"; // the id string of the RootIdentity
String storePasswd = "secret";
String exportPasswd = "passwd"
File exportFile = new File("/tmp/export-sample.json");

store.exportRootIdentity(id, exportFile, exportPasswd, storePasswd);
```

为了满足不同的应用需求，RootIdentity export的输出目标可以是 File、OutputStream、Write 等对象，详细的方法参见 [Java API 文档](https://todo/url/to/javadoc)。

The output targets of Root Identity export can be File, Output Stream, Write and other objects in order to meet different application requirements. See Java API documentation for detailed methods.

## 导入 RootIdentity

Import Root Identity

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
String exportPasswd = "passwd"
File exportFile = new File("/tmp/export-sample.json");

store.importRootIdentity(exportFile, exportPasswd, storePasswd);
```

和 RootIdentity 导出一样，RootIdentity import 的输入也可以是 File、InputStream、Reader 等对象，详细的方法参见 [Java API 文档](https://todo/url/to/javadoc)。

As with Root Identity export, the input of Root Identity import can be objects, such as File, Input Stream and Reader. See Java API documentation for detailed methods.

如果 DIDStore 中已经存在导出文件对应的 RootIdentity，那么 import 操作将覆盖目标 store 中 RootIdentity 现有的数据。

If the Root Identity corresponding to the exported file already exists in DID Store, then the import operation will overwrite the existing data of Root Identity in the target store.

> RootIdentity 也可以通过助记词进行备份和恢复，详细参见 [RootIdentity 的备份和恢复](../rootidentity/backup-restore-rootidentity.md)。
>
> Root Identity can also be backed up and restored by mnemonics. For details, see backup and recovery of Root Identity.
