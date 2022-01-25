# Export/Import RootIdentity

DID Store offers a method of exporting the specified RootIdentity, including the id, mnemonic, private key, derive index, metadata, and more of RootIdentity.

The exported result is a string in the format of json, and the exported content of RootIdentity is stored in json one-by-one in the form of json fields. For security reasons, the private key is encrypted.

DID Store provides a method of importing RootIdentity, which directly saves json data in the imported DID Store as RootIdentity object, thereby accomplishing the migration of RootIdentity.

## Example

```typescript
let rootPath = "root/store";
let restorePath = "root/newStore";
let store = await DIDStore.open(rootPath);
... ... ... ...
let ids = await store.listRootIdentities();
let id = ids[0].getId();
... ... ... ...  
//export RootIdentity
let data = await store.exportRootIdentity(id, "password", "pwd");
store.close();
if (data != null && data !== "") {
    console.log("export root identity {} successfully.", id);
    let newStore = await DIDStore.open(restorePath);
    //import RootIdentity
    await newStore.importRootIdentity(data, "password", "newpwd");
    let rootId = newStore.loadRootIdentity(id);
    if (rootId)
        console.log("import root identity {} successfully.", id);
    else
        console.log("import root identity {} failed.", id);
    ... ... ... ...
    newStore.close();
} else {
    console.log("export root identity {} failed.", id);
}
```

## Usage

```typescript
public exportRootIdentity(
    id: string,
    password: string,
    storepass: string
): string;
```

id is the identifier that needs to export RootIdentity; password is the export password used for the encryption of RootIdentity private key, which is required when importing data; storepass is the store password for exporting DID Store, and users obtain the private key from there.

```typescript
public importRootIdentity(
    data: string,
    password: string,
    storepass: string
): void;
```

Data refers to information in json format, or the exported data; password is the import password, which is consistent with the export password. If it's incorrect, the import function cannot be completed correctly. Storepass is the store password imported into the DID Store, which is used for the encryption of private key within. There's no need to provide any information about the id identifier of RootIdentity.
