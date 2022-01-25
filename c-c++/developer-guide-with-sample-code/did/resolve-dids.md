# Resolve DIDs

The method provided by DID is used to obtain DID Document or historical DID Document and complete a validity check.

Resolve has two ways to obtain DID Document: one is Chain Resolve, and the return is DID Document existing on the chain - the other is Local Resolve. Sometimes, several DID contents only need to exist without being on the chain due to some special circumstances. If the Resolve function is needed to obtain the use of DID Document in the follow-up work, Local Resolve can be utilized. Some users provide DID Document that needs to be returned - this method is mainly used for verification.

Getting DID historical transactions can only be done through Chain Resolve.

In order to increase the flexibility, the DID Document obtained through resolve needs users to save it to the DID store and update it locally.

## Chain Resolve

Before using the Chain Resolve method, make sure that DID Backend has been initialized and the chain address of resolve has been provided; otherwise resolve will fail.

There are three states of DID on the chain: not found (not found on the chain, which means that there is no DID on the chain), valid. and deactivated.

### Example

```c
bool createIdTransaction(const char *payload, const char *memo)
{
    if (!payload)
        return false;

    ... ... ... ...
    return true;
}

const char *resolver = "testnet";
... ... ... ... 
if (DIDBackend_InitializeDefault(createIdTransaction, resolver, cachedir) < 0)
  	printf("initialize DIDBackend failed.\n");

DID *did = DID_FromString("did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y");
int status = 0;
if (did) {
    //resolve did
    DIDDocument *doc = DID_Resolve(did, &status, true);
    if (doc) {
        console.log("resolve did {} successfully.", did);
        //get information from DIDDocument
        DIDDocument_Destroy(doc);
    } else {
        cosole.log("resolve did {} failed, status: {}", did, status);
    }
    ... ... ...
    //resolve did history
    DIDBiography *bio = DID_ResolveBiography(did);
    if (bio) {
        console.log("resolve did biography: count={}", bio.getTransactionCount());
        ... ... ... ...
        //get information from DIDBiography
        DIDBiography_Destroy(bio);
    } else {
        console.log("resolve did biography failed.");
    } 
    DID_Destroy(did);
}
```

### Usage

```c
DIDDocument *DID_Resolve(DID *did, DIDStatus *status, bool force);
```

Resolve gets the latest (or the last) DID Document on the chain. If it exists on the chain, the network is normal, DID Document verification is valid, and DID Document object is returned; otherwise, NULL is returned, and the status of DID failure can be obtained by status.

force indicates whether it's necessary to get DID Document from the chain. There is a time-limited cache (10 minutes) for SDK resolve results. If force is false and the cache is within the time limit, resolve returns to the cache result; if force is false but the cache is in effect (or if force is true), the DID Document data will be obtained directly from the chain.

```c
DIDBiography *DID_ResolveBiography(DID *did);
```

This method can obtain DID history information. If it fails, NULL is returned; if it succeeds, DID Biography object is returned. DID Biography contains DID status and the content of each transaction. See detailed API documentation.

## Local Resolve

### Example

```c
bool createIdTransaction(const char *payload, const char *memo)
{
    if (!payload)
        return false;

    ... ... ... ...
    return true;
}

DIDDocument *local_doc(DID *did)
{
    ... ... ... ...
    return doc;
}

const char *resolver = "testnet";
... ... ... ... 
if (DIDBackend_InitializeDefault(createIdTransaction, resolver, cachedir) < 0)
  	printf("initialize DIDBackend failed.\n");

DID *did = DID_FromString("did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y");
int status = 0;
if (did) {
    //resolve did from 'local_doc' interface, enable resolve handle
    DIDBackend_SetLocalResolveHandle(local_doc);
    DIDDocument *doc = DID_Resolve(did, &status, true);
    if (doc) {
        console.log("resolve did {} successfully.", did);
        //get information from DIDDocument
        DIDDocument_Destroy(doc);
    } else {
        cosole.log("resolve did {} failed, status: {}", did, status);
    }
    //disable resolve handle
    DIDBackend_SetLocalResolveHandle(NULL);
    ... ... ...
    //resolve did history from chain
    DIDBiography *bio = DID_ResolveBiography(did);
    if (bio) {
        console.log("resolve did biography: count={}", bio.getTransactionCount());
        ... ... ... ...
        //get information from DIDBiography
        DIDBiography_Destroy(bio);
    } else {
        console.log("resolve did biography failed.");
    } 
    DID_Destroy(did);
}
```

### Usage

```c
typedef DIDDocument* DIDLocalResovleHandle(DID *did);

void DIDBackend_SetLocalResolveHandle(DIDLocalResovleHandle *handle);
```

DIDLocalResolveHandle is implemented by the local DID Document interface provided by the user, instead of getting DID Document from the chain.

handle is NULL, which means that DID Document is obtained from the chain without Local resolve. When you need to use Local resolve, just implement the Local Resolve Handle category.
