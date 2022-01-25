# Export/Import Root Identity

DID Store object provides a method to export and import Root Identity, which can export mnemonics, public/private keys, and other data related to a specific Root Identity to a JSON file. The Private Key in the export file is encrypted and saved, and the key is the passphase of the export file set at the time of export. The exported JSON is an autocorrelation single file without any dependence, and the application can easily use the exported file to migrate the data of Root Identity.

## Export Root Identity

```java
DIDStore store; // an opened DIDStore instance
String id = "d2f3c0f07eda4e5130cbdc59962426b1"; // the id string of the RootIdentity
String storePasswd = "secret";
String exportPasswd = "passwd"
File exportFile = new File("/tmp/export-sample.json");

store.exportRootIdentity(id, exportFile, exportPasswd, storePasswd);
```

The output targets of Root Identity export can be File, Output Stream, Write and other objects in order to meet different application requirements. See [Java API documentation](../../../concepts/did-and-document.md) for detailed methods.

## Import Root Identity

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
String exportPasswd = "passwd"
File exportFile = new File("/tmp/export-sample.json");

store.importRootIdentity(exportFile, exportPasswd, storePasswd);
```

As with Root Identity export, the input of Root Identity import can be objects, such as File, Input Stream, and Reader. See [Java API documentation](https://todo/url/to/javadoc) for detailed methods.

If the Root Identity corresponding to the exported file already exists in DID Store, then the import operation will overwrite the existing data of Root Identity in the target store.

> Root Identity can also be backed up and restored by mnemonics. For details, see backup and recovery of Root Identity.
