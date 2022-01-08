# Access DIDStore

## Read Objects in DID Store

DID Store provides a series of load API for reading objects, which can support reading Root Identity, DID Document, and Verifiable Credential within.

However, for security reasons, the private key stored in DID Store cannot be read directly but can only be used transparently through the signature or encrypted API.

* #### Example

```c
const char *rootPath = "root/store";
DIDStore *store = DIDStore_Open(rootPath);

// Load RootIdentity
RootIdentity *rootidentity = DIDStore_LoadDefaultRootIdentity(store);
if (rootidentity) {
    ... ... ...
    RootIdentity_Destroy(rootidentity);
}

DID *did = new DID("did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y");
// Load DIDDocuemnt
DIDDocument *doc = DIDStore_LoadDid(store, did);
... ... ... ...
DID_Destroy(did);
DIDDocument_Destroy(doc);

DIDURL *id = DIDURL_NewFromDid(did, "cred-1");
// Load VerifiableCredential
Credential *vc = DIDStore_LoadCredential(store, DIDURL_GetDid(id), id);
... ... ... ...
DIDURL_Destroy(id);
Credential_Destroy(vc);
... ... ... ...
DIDStore_Close(store);
```

* #### Usage

```c
RootIdentity *DIDStore_LoadRootIdentity(DIDStore *store, const char *id);
```

id is the identifier string of Root Identity. If the DID Store has only one Root Identity, the api can be used without ID identification, as the example above (or the acquisition will fail).

The result is Root Identity object. **Attention:** The Root Identity object needs to be destroyed with RootIdentity\_Destroy after it's used.

```c
DIDDocument *DIDStore_LoadDID(DIDStore *store, DID *did);
```

If the DID content doesn't exist or the DID Document is saved in the wrong format, the acquisition fails. **Note:** There are many ways to get DID object, and the example is just one of them. Users can use API according to their own needs. See DID/Create DID and DID Document chapter for details.

The result is DID Document object (also the DID content saved by DID Store). **Attention:** The Root Identity object needs to be destroyed with RootIdentity\_Destroy after it's used.

```c
Credential *DIDStore_LoadCredential(
    DIDStore *store,
    DID *did,
    DIDURL *credid);
```

did is the DID belonging to Credential; credid is the unique identifier of Credential. If the credential does not exist or the format is incorrect, the acquisition fails. **Note:** There are many ways to get DIDURL object, the example is just one of them, and users can use API according to their own needs. See DID/Create DID and DID Document section for details.

The result is Credential object. **Attention:** The Credential object needs to be destroyed with Credential\_Destroy after it is used.

## Save DID Information in DID Store

DID Store provides a series of APIs for saving DID information, mainly including DID Document, Verifiable credential, and Private key.

Among them, saving the private key is mainly used to add the public key in the key pair to the Document when DID Document adds the key, and the private key is encrypted and saved to the DID Store, which is used for authorization and entrustment of DID.

* #### Example

```c
const char *rootPath = "root/store";
DIDStore *store = DIDStore_Open(rootPath);
... ... ... ...
DID *did = new DID("did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y");
int status = 0;
DIDDocument *doc = DID_Resolve(did, &status, true);
if (doc) {
    // store DIDDocument
    DIDStore_StoreDid(store, doc);
  	DIDDocument_Destroy(doc);
}
... ... ... ...
const char *storePass = "pwd";
DIDURL *id = DIDURL_NewFromDid(did, "key-1");
//store PrivateKey
if (DIDStore_StorePrivateKey(store, id, sk, storePass) < 0)
  	//error operation
DIDURL_Destroy(id);

... ... ... ...
DIDURL *credid = DIDURL_NewFromDid(did, "mycredential");
Issuer *issuer = Issuer_Create(did, NULL, store);
const char *types[] = {"BasicProfileCredential", "PhoneCredential",
                       "SelfProclaimedCredential"};
Property props[2];
props[0].key = "name";
props[0].value = "John";
props[1].key = "gender";
props[1].value = "Male";

Credential *vc = Issuer_CreateCredential(issuer, did, credid, types, 3, props, 2,
        expires, storePass);
Issuer_Destroy(issuer);
DIDURL_Destroy(credid);
if (vc) {
    //store Credential
    DIDStore_StoreCredential(store, vc);
  	Credential_Destroy(vc);
}
DID_Destroy(did);
... ... ... ...
DIDStore_Close(store);
```

* #### Usage

```c
int DIDStore_StoreDID(DIDStore *store, DIDDocument *document);
```

The method saves DID Document in DID Store.

```c
int DIDStore_StoreCredential(DIDStore *store, Credential *credential);
```

The method saves Credential object in DID Store.

```c
int DIDStore_StorePrivateKey(
        DIDStore *store,
        const char *storepass,
        DIDURL *id,
        const uint8_t *privatekey,
        size_t size);
```

storepass is used to encrypt and save the private key in DID Store; privateKey __ is an 82-bit extended private key; size is the length of the private key.

## Enumerate Objects in DID Store

DID Store is a background store, and users need to know all DID information saved winth it to get it by enumerating. DID Store provides examples of Root Identity, DID, and Verifiable Credential.

For security reasons, the private key cannot be enumerated.

* #### Example

```c
int get_rootidentity(RootIdentity *rootidentity, void *context)
{
    int *count = (int*)context;

    if (!rootidentity)
        return 0;

    (*count)++;
    return 0;
}

int get_did(DID *did, void *context)
{
    DID *d = (DID*)context;

    if (!did)
        return 0;

    (*count)++;
    return 0;
}

const char *rootPath = "root/store";
DIDStore *store = DIDStore_Open(rootPath);
... ... ... ...

//list RootIdentities
int count = 0;
if (DIDStore_ListRootIdentities(store, get_rootidentity, (void*)&count) < 0)
  //error operation
  
if (count > 0) 
    printf("there are %d root identities in the DID store.\n", count);
else
    printf("there are no root identity in the DID store.\n");
... ... ... ...   

//list DIDs
count = 0;
if (DIDStore_ListDIDs(store, 0, get_did, (void*)&count) < 0)
  //error operation
  
if (count > 0)
    printf("there are %d dids in the DID store.\n", count);
... ... ... ...

DIDStore_Close(store);
```

* #### Usage

```c
typedef int DIDStore_RootIdentitiesCallback(RootIdentity *rootidentity, void *context);

ssize_t DIDStore_ListRootIdentities(
        DIDStore *store,
        DIDStore_RootIdentitiesCallback *callback,
        void *context)
```

Enumerate the Root Identity saved in DID Store and pass the Root Identity object to the user through callback.

```c
typedef int DIDStore_DIDsCallback(DID *did, void *context);
typedef enum {
    DIDFilter_All = 0,
    DIDFilter_HasPrivateKey = 1,
    DIDFilter_WithoutPrivateKey = 2
} ELA_DID_FILTER;

int DIDStore_ListDIDs(
        DIDStore *store,
        ELA_DID_FILTER filer,
        DIDStore_DIDsCallback *callback,
        void *context);
```

Enumerate DID saved in DID store and pass DID to the user through callback. The filer is a DID filter, which is used to indicate the conditions for selecting DID.

```c
typedef int DIDStore_CredentialsCallback(DIDURL *id, void *context);

int DIDStore_ListCredentials(
        DIDStore *store,
        DID *did,
        DIDStore_CredentialsCallback *callback,
        void *context);
```

Enumerate all credentials of the specified DID and pass the credential ID Enumerate DID saved in DID store, then pass credential ID to the user through callback.

## Select Objects in DID Store

The List function can list all objects of the same type in DID Store, and the select function can list DID and Verifiable Credential objects that meet the specified conditions.

* #### Example

```c
const char *rootPath = "root/store";
DIDStore *store = DIDStore_Open(rootPath);
... ... ... ...
Credential *vcs[1];
//select Credentials
if (DIDDocument_SelectCredentials(customized_doc, "SelfProclaimedCredential",
            NULL, vcs, sizeof(vcs) < 0)
 		//error operation
... ... ... ...
DIDStore_Close(store);
```

* #### Usage

```c
ssize_t DIDDocument_SelectCredentials(
        DIDDocument *document,
        const char *type,
        DIDURL *credid,
        Credential **creds,
        size_t size);
```

This method selects the eligible Credential method according to type and credid, and then returns the eligible Credential Id and puts it in the creds array.

creds is an array provided by the user to store the Id of eligible Credential, and size is the array size. There must be one value exists in type and credid, otherwise an error will be reported.

## Delete Objects in DID Store

DID Store provides a method to save DID objects as well as a method to delete them, which need to delete Root Identity, DID, Verifiable Credential, and Private key consecutively.

* #### Usage

```c
bool DIDStore_DeleteRootIdentity(DIDStore *store, const char *id);
```

This method is used to delete the Root Identity identified as id. If there is no such Root Identity, or the Root Identity is the only object in DID store, then it cannot be deleted; return to false.

```c
bool DIDStore_DeleteDID(DIDStore *store, DID *did);
```

This method is used to delete all did-related contents saved in DID Store, such as documents, credentials, and private keys. If there is no DID information or an error occurs, it will return to false.

```c
bool DIDStore_DeleteCredential(DIDStore *store, DID *did, DIDURL *id);
```

This method is used to delete Credential. If there is no Credential information or an error occurs, it will return to false.

did is the owner of Credential, and id is Credential identification.

```c
void DIDStore_DeletePrivateKey(DIDStore *store, DIDURL *keyid);
```

This method provides the private key for deleting keyid. If there is no private key information or an error occurs, it will return to false.

## Check whether there are Objects in DID Store

Sometimes users only need to know whether an object is saved in DID store. Therefore, DID store provides a method to check the existence of Root Identity, Mnemonic, DID, Verifiable Credential, and Private key.

* #### Usage

```c
int DIDStore_ContainsRootIdentity(DIDStore *store, const char *id);
```

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, didstore doestn't contain rootidentiy;
  ```
* ```
   return value = 1, didstore contains rootidentiy.
  ```

```c
int DIDStore_ContainsRootIdentities(DIDStore *store);
```

Check whether DID Store contains Root Identity.

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, there isn't rootidentity in didstore;
  ```
* ```
   return value = 1, there is rootidentity in didstore.
  ```

```c
int DIDStore_ContainsRootIdentityMnemonic(DIDStore *store, const char *id);
```

Check whether the specified Root Identity contains mnemonic.

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, there isn't rootidentity's mnemonic in didstore;
  ```
* ```
   return value = 1, there is rootidentity's mnemonic in didstore.
  ```

```c
int DIDStore_ContainsDID(DIDStore *store, DID *did);
```

Check whether the DID Store contains the specified DID.

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, did isn't in didstore;
  ```
* ```
   return value = 1, did is in didstore.
  ```

```c
int DIDStore_ContainsDIDs(DIDStore *store);
```

Check whether DID Store contains DID.

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, no did isn't in didstore;
  ```
* ```
   return value = 1, did is in didstore.
  ```

```c
int DIDStore_ContainsCredential(DIDStore *store, DID *did, DIDURL *credid);
```

Check whether DID store contains Credential with the specified id.

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, credential isn't in didstore;
  ```
* ```
   return value = 1, credential is in didstore.
  ```

```c
int DIDStore_ContainsCredentials(DIDStore *store, DID *did);
```

Check whether DID store contains Credential with the specified DID.

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, there isn't credential in didstore;
  ```
* ```
   return value = 1, there is credential in didstore.
  ```

```c
int DIDStore_ContainsPrivateKey(DIDStore *store, DID *did, DIDURL *keyid);
```

Check whether DID store contains the Private key with the specified id.

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, there isn't private key in didstore;
  ```
* ```
   return value = 1, there is private key in didstore.
  ```

```c
int DIDSotre_ContainsPrivateKeys(DIDStore *store, DID *did);
```

Check whether the DID store contains the private key with the specified did.

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, there isn't private key in didstore;
  ```
* ```
   return value = 1, did is deacativated.
  ```
