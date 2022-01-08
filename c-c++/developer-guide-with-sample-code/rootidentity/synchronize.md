# Backup and Synchronize

Root Identity provides a method to get DIDs on all chains derived from Root Identity and saves the corresponding DID Documents in DID Store.

This method is convenient for the user to update multiple DIDs under the root identity at one time, retrieve the latest document, and initialize the user’s new DID Store.

### Derive Mnemonic

```c
const char *rootPath = "root/store";
DIDStore *store = await DIDStore.open(rootPath);
... ... ... ...
const char *mnemonic = "pact reject sick voyage foster fence warm luggage cabbage any subject carbon";
RootIdentity *identity = RootIdentity_Create(mnemonic, "", store, "pwd", true);
const char *id = RootIdentity_GetId(identity);

char exportMnemonic[ELA_MAX_MNEMONIC_LEN + 1];
if (DIDStore_ExportRootIdentityMnemonic(store, "pwd",
            id, exportMnemonic, sizeof(exportMnemonic)) < 0)
   //error operation
   
 RootIdentity_Destroy(identity);
 ... ... ... ...
 DIDStore_Close(store);
```

### Restore RootIdentity

```c
const char *rootPath = "root/store";
DIDStore *store = await DIDStore.open(rootPath);
... ... ... ...
const char *mnemonic = "pact reject sick voyage foster fence warm luggage cabbage any subject carbon";
RootIdentity *identity = RootIdentity_Create(mnemonic, "", store, "pwd", true);
if (!identity)
  	//error operation
  
printf("Synchronizing from IDChain...\n");
//synchronize
if (!RootIdentity_Synchronize(identity, NULL))
  	//error operation
  
... ... ... ...
RootIdentity_Destroy(identity);
... ... ... ...
DIDStore_Close(store);
```

## Usage

```c
bool RootIdentity_Synchronize(
    RootIdentity *rootidentity,
    DIDDocument_ConflictHandle *handle);
```

handle is a solution provided by users when local and chain-acquired DID Document conflict. The SDK's default solution is to keep local DID Document and ignore the version of the chain. If users have different solutions, this parameter needs to be set.

```c
bool RootIdentity_SynchronizeByIndex(
        RootIdentity *rootidentity,
        int index,
        DIDDocument_ConflictHandle *handle);
```

index indicates which DID is derived from the root identity; the meaning of handle is as above.
