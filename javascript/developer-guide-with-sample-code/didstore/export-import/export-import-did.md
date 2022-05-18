# Export/Import DID

DID Store provides a method to export the specified DID, with the exported content mainly including DID, DID document, verifiable credentials (if any), and private keys (if any).

The exported result is a string in JSON format, and the exported content of DID is orderly stored in JSON by JSON field. For security reasons, the private key is stored encrypted.

DID Store provides the method of importing DID by directly saving JSON data as a DID object in the imported DIDstore, thereby completing the migration of DID.

## Example

```typescript
let rootPath = "root/store";
let restorePath = "root/newStore";
let store = await DIDStore.open(rootPath);
... ... ... ...
let dids = await store.listDids();
let did = dids[0];
... ... ... ...  
//export DID
let data = await store.exportDid(did, "password", "pwd");
store.close();

if (data != null && data !== "") {
    console.log("export DID {} successfully.", did);
    let newStore = await DIDStore.open(restorePath);
    //import DID
    await newStore.importDid(data, "password", "newpwd");
    let doc = newStore.loadDid(did);
    if (doc)
        console.log("import DID {} successfully.", did);
    else
        console.log("import DID {} failed.", did);
    newStore.close();
} else {
    console.log("export DID {} failed.", did);
}
```

## Usage

```typescript
public async exportDid(
    did: DID | string,
    password: string,
    storepass: string
): Promise<string>;
```

Password refers to the export password used for the encryption of DID private key, which needs to be used when importing data; storepass is the store password for exporting DID Store, and users get the private key from DID Store.

```typescript
public importDid(
    data: string,
    password: string,
    storepass: string
): void;
```

Data refers to information in JSON format, that is, the exported data; password is the import password, which is consistent with the export password. If it is incorrect, the import function cannot be completed correctly. Storepass is the store password imported into the DID Store, which is used for the encryption of private key in the DID Store.
