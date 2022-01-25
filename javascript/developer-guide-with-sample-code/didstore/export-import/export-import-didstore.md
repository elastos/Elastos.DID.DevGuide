# Export/Import DIDStore

DID Store provides a method to export the entire DIDStore - RootIdentity and DID are the exported content.

The exported result is a zip file. To get multiple RootIdentities and DIDs from the DID Store, users need to obtain data files according to the export methods in the previous two chapters, then save them in a .zip file.

DID Store provides a method of importing by directly doing so with .zip files and saving them as RootIdentity and DID objects - this completes the migration of the whole DID Store.

## Example

```typescript
let rootPath = "root/store";
let restorePath = "root/newStore";
let store = await DIDStore.open(rootPath);
... ... ... ... 
//export DID store
let tempDir = new File(TestConfig.tempDir);
tempDir.createDirectory();
let exportFile = new File(tempDir, "storeexport.zip");
await store.exportStore(exportFile.getAbsolutePath(), "password", "pwd");
store.close();
let newStore = await DIDStore.open(restorePath);
//import DID store
await newStore.importStore(exportFile.getAbsolutePath(), "password", "newpwd");
newStore.close();
```

## Usage

```typescript
public async exportStore(
    zipFile : string,
    password: string,
    storepass: string
): Promise<void>;
```

Password is the export password used for encrypting the private key of DID, which is required when importing data; storepass is the store password for exporting DID Store, and users obtain the private key from the DID Store.

```typescript
public async importStore(
    zipFile: string,
    password: string,
    storepass: string
): Promise<void>;
```

zipFile is the zip file path; password is the import password, which is consistent with the export password. If it's incorrect, the import function cannot be completed correctly. Storepass is the store password for importing DID Store, which is used for the encryption of the private key in the DID Store.
