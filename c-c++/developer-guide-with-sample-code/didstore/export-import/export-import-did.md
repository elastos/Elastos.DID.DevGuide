# Export/Import DID

DID store provides a method to export the specified DID, which mainly includes DID, DID Document, Verifiable Credentials (if any). and private keys (if any).

The Export result is a string in json format, and the related export contents of DID are saved in json in the form of json fields successively. For security reasons, the private key is encrypted and saved.

DID store provides the method of importing DID and directly saves json data in the imported DID store as a DID object to complete the migration of DID.

## Example

```c
//export DID
const char *filepath = "/did/context.json";
if (DIDStore_ExportDID(store, storePass, did, filepath, "pwd") < 0)
  //error operation
if (DIDStore_ImportDID(newStore, newStorePass, filepath, "pwd") < 0)
  //error operation
//do something about did in new DID Store
... ... ... ...
```

## Usage

```c
int DIDStore_ExportDID(
        DIDStore *store,
        const char *storepass,
        DID *did,
        const char *file,
        const char *password);
```

storepass is the store pass for exporting DID Store, and the user obtains the private key from DID Store; file is the file path provided by the user to save the export result, which may not exist; password is the export password, which is used for DID private key encryption and needs to be used during the data import.

```c
int DIDStore_ImportDID(
        DIDStore *store,
        const char *storepass,
        const char *file,
        const char *password);
```

file is the file path provided by the user to import DID content; password is the import password, which is the same as the export password. If it's incorrect, the import function cannot be completed correctly. storepass is the store pass imported into DID store, which is used to encrypt the private key in DID Store.
