# Export/Import DIDStore

DID Store provides a method to export everything in its entirety. The exported content has two parts: Root Identity and DID.

The Export result is a .zip file. Get multiple Root Identity and DIDs in DID Store, retrieve data files according to the export method in the previous two section, and save them in a .zip file.

DID Store provides the method of importing DID Store, directly importing .zip files and saving them in the imported DID Store with Root Identity and DID objects, thus completing the migration of the whole DID Store.

## Example

```c
//export DID Store
const char *zipfile = "/did/store.zip";
if (DIDStore_ExportStore(store, storePass, zipfile, "pwd") < 0)
		//error operation
  
if (DIDStore_ImportStore(newStore, newStorePass, zipfile, "pwd") < 0)
		//error operation
  
//do something in the new DID Store
... ... ... ...
```

## Usage

```c
int DIDStore_ExportStore(
        DIDStore *store,
        const char *storepass,
        const char *zipfile,
        const char *password);
```

This method provides the export method of the whole DID Store. store is the DID Store to be exported; storepass is the store pass of store; zipfile __ is the path of the exported zip file; password __ is the export password, used for DID private key encryption required when importing data.

```c
int DIDStore_ImportStore(
        DIDStore *store,
        const char *storepass,
        const char *zipfile,
        const char *password);
```

This method provides an import method for the whole DID Store. store is the imported new DID Store; storepass s the password of the store, which is used to encrypt the imported private key;  zipfile is the path to import zip file; password is the import password, which is the same with the export password. If it's wrong, the import function cannot be completed correctly.
