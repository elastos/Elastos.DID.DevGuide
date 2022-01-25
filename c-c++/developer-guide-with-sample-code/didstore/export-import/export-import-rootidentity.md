# Export/Import RootIdentity

DID Store provides a method to export the specified Root Identity, including its ID and the mnemonic, private key, derive index, metadata, etc.

The Export result is a string in json format, and the related export contents of Root Identity are stored in json in the form of json fields successively. For security reasons, the private key is encrypted and saved.

DID Store provides a method of importing Root Identity, which directly saves json data in the imported DID Store as Root Identity object and completes the migration of Root Identity.

## Example

```c
//export RootIdentity
const char *filepath = "/did/default-rootidentity.json";
if (DIDStore_ExportRootIdentity(store, storePass, id, filepath, "pwd") < 0)
  //error operation

if (DIDStore_ImportRootIdentity(newStore, newStorePass, filepath, "pwd") < 0)
  //error operation
  
//do something about RootIdentity in new DID Store
... ... ... ...
```

## Usage

```c
int DIDStore_ExportRootIdentity(
        DIDStore *store,
        const char *storepass,
        const char *id,
        const char *file,
        const char *password);
```

id is the identifier that needs to export Root Identity; password is the export password, used for Root Identity private key encryption, which is required when importing data; storepass is the store password for exporting DID store, and the user gets the private key from DID store.

```c
int DIDStore_ImportRootIdentity(
        DIDStore *store,
        const char *storepass,
        const char *file,
        const char *password);
```

data is the data in json format, or export data; password is the import password, which is the same as the export password. If it's wrong, the import function cannot be completed correctly. storepass is the store password imported into DID store, which is used for encryption of private key in DID store. There is no need to provide id identifier information of Root Identity.
