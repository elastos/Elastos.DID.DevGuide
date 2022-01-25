# Synchronize

DID Store provides a method to update all DID objects within at once, as well as obtain and save the most suitable DID Document object.

This method will not only synchronize all the Root Identity in the DID Store, but also synchronize the customized DID saved in the DID Store so as to realize full DID synchronization within.

## Example

```c
const char *rootPath = "root/store";
DIDStore *store = DIDStore_Open(rootPath);
... ... ... ...
//Synchronize
DIDStore_Synchronize(store, NULL);
//to check DID object or use the newest DID Document
... ... ... ...
DIDStore_Close(store);
```

## Usage

```c
void DIDStore_Synchronize(
    DIDStore *store,
    DIDDocument_ConflictHandle *handle);
```

handle is a solution provided by users when local and chain-acquired DID Document conflict. The default solution provided by the SDK is to keep the local DID Document and ignore the version of the chain. If users have different solutions, this parameter needs to be set.
