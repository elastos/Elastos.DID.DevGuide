# Export/Import DID Store

DID Store object provides a method to export and import the whole store, which can export all the Root Identity, DID documents, verifiable credentials, private keys, and other related data in DID Store to one file. The Private Key in the export file is encrypted and saved, and the key is the passphase of the export file set at the time of export. The exported file is an autocorrelation single ZIP file without any dependence, and the application can easily use the exported file to migrate or backup/reply to DID Store.

## Export Store

```java
DIDStore store; // an opened DIDStore instance

String storePasswd = "secret";
String exportPasswd = "passwd"
File exportFile = new File("/tmp/export-sample.zip");

store.exportStore(exportFile, exportPasswd, storePasswd);
```

To meet different application requirements, the output target of DID Store export can be File, Zip Output Stream, and other objects. See [Java API documentation](https://todo/url/to/javadoc) for detailed methods.

## Import Store

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
String exportPasswd = "passwd"
File exportFile = new File("/tmp/export-sample.zip");

store.importStore(exportFile, exportPasswd, storePasswd);
```

As with Store export, the input of DID Store import can also be File, Zip Input Stream and other objects. See J[ava API documentation](https://todo/url/to/javadoc) for detailed methods.

If the object corresponding to the exported file already exists in DID Store, the import operation will overwrite the existing object data in the target store.
