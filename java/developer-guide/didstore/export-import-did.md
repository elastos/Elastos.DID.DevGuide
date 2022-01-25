# Export/Import DID

DID Store object provides a method to export and import DID, which can export data related to a specific DID, such as DID documents, verifiable credentials, and private keys to a JSON file. The Private Key in the export file is encrypted and saved, and the key is the passphase of the export file set at the time of export. The exported JSON is an autocorrelation single file without any dependence, and the application can easily use the exported file to migrate DID data.

## Export DID

```java
DIDStore store; // an opened DIDStore instance
DID did = new DID("did:elastos:ibeTptCepbUFE8qjmAUmQGp1eRuNnXZosL");
String storePasswd = "secret";
String exportPasswd = "passwd"
File exportFile = new File("/tmp/export-sample.json");

store.exportDid(did, exportFile, exportPasswd, storePasswd);
```

To meet different application requirements, DID export’s output targets can be File, Output Stream, Write, and other objects. See [Java API documentation](https://todo/url/to/javadoc) for detailed methods.

## Import DID

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
String exportPasswd = "passwd"
File exportFile = new File("/tmp/export-sample.json");

store.importDid(exportFile, exportPasswd, storePasswd);
```

As with DID export, the input of DID import can also be File, Input Stream, Reader, and other objects. See [Java API documentation](https://todo/url/to/javadoc) for detailed methods.\\

If the DID corresponding to the exported file already exists in the DID Store, the import operation will overwrite the existing data of DID in the target store.
