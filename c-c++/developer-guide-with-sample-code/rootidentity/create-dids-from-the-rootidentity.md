# Create DIDs from the RootIdentity

## Example

```c
const char *rootPath = "root/store";
DIDStore *store = DIDStore_Open(rootPath);
... ... ... ...
const char *mnemonic = "pact reject sick voyage foster fence warm luggage cabbage any subject carbon";
const char *passphrase = "helloworld";
RootIdentity *identity = RootIdentity_Create(mnemonic, passphrase, true, store, storepass);
if (!identity)
  	//error operation

//create a new DID
DIDDocument *doc = RootIdentity_NewDID(identity, storepass, "", true);
if (!doc)
  	//error operation

DID *did = RootIdentity_GetDIDByIndex(identity, 0);
if (!did)
  	//error operation
  
if (DID_Equals(did, DIDDocument_GetSubject(doc)) != 1)
  	//error operation
  
DID_Destroy(did);
DIDDocument_Destroy(doc);
RootIdentity_Destroy(identity);
... ... ... ...
DIDStore_Close(store);
```

## Usage

```c
DIDDocument *RootIdentity_NewDID(
        RootIdentity *rootidentity,
        const char *storepass,
        const char *alias,
        bool overwrite);
```

The method provides a new DID derived from Root Identity, saves its DID Document in DID Store, and returns to DID Document.

It's worth noting that this function doesn't need to provide a DID Store, which is provided by Root Identity (the associated DID Store is recorded by Root Identity during the generation).

overwrite indicates whether the existing DID needs to be overwritten. When overwrite is true, it will overwrite the existing DID and return to the newly generated DID Document object; when overwrite is false, NULL is returned if DID already exists.

```c
DIDDocument *RootIdentity_NewDIDByIndex(
        RootIdentity *rootidentity,
        int index,
        const char *storepass,
        const char *alias,
        bool overwrite);
```

The method derives the index DID of Root Identity - its parameters are the same as RootIdentity\_NewDID.

```c
void DIDDocument_Destroy(DIDDocument *document);
```

If the generated DID Document is not used, it needs to be destroyed with this method.

```c
DID *RootIdentity_GetDIDByIndex(RootIdentity *rootidentity, int index);
```

This method is used to get the DID object, and DID Document is not generated or saved. It is mainly used for users to obtain DID objects to perform other DID operations.
