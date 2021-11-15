# Export/import DIDStore

DIDStore 对象提供了对整个 store 进行导出/导入的方法，可以实现将 DIDStore 中所有的 RootIdentity、DID 文档、可验证凭证、私钥以及其他相关的数据导出到一个文件中。在导出文件中的 PrivateKey 是加密保存的，密钥为导出时设定的导出文件密码。导出的文件是一个没有任何依赖的自相关单一 ZIP 文件，应用可以方便的使用导出文件迁移或者备份/回复 DIDStore。

## 导出 Store

```java
DIDStore store; // an opened DIDStore instance

String storePasswd = "secret";
String exportPasswd = "passwd"
File exportFile = new File("/tmp/export-sample.zip");

store.exportStore(exportFile, exportPasswd, storePasswd);
```

为了满足不同的应用需求，DIDStore export的输出目标可以是 File、ZipOutputStream 等对象，详细的方法参见 [Java API 文档](https://todo/url/to/javadoc)。

## 导入 Store

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
String exportPasswd = "passwd"
File exportFile = new File("/tmp/export-sample.zip");

store.importStore(exportFile, exportPasswd, storePasswd);
```

和 Store 导出一样，DIDStore import 的输入也可以是 File、ZipInputStream 等对象，详细的方法参见 [Java API 文档](https://todo/url/to/javadoc)。

如果 DIDStore 中已经存在导出文件对应的对象，那么 import 操作将覆盖目标 store 中现有的对象数据。

