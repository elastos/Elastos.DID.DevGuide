# Resolve DIDs

DID提供方法用于获取DID Document或者历史DID Document，并完成DID Document有效性检查。

The method provided by DID is used to obtain DID Document or historical DID Document and complete the validity check of it.

Resolve有两种方式获取DID Document，一种是Chain Resolve，返回是链上存在的DID Document；另一种是Local Resolve，因为一些特殊情况，有时某些DID内容只需要存在但是并不需要上链，但是后续工作中又需要用到Resolve功能来获取DID Document使用，这个时候就可以用到Local Resolve。有用户提供需要返回的DID Document，这种方法主要用于验签上。

Resolve has two ways to obtain DID Document, one is Chain Resolve, and the return is DID Document existing on the chain; The other is Local Resolve. Sometimes, several DID contents only need to exist without being on the chain due to some special circumstances. If the Resolve function is needed to obtain the use of DID Document in the follow-up work, Local Resolve can be utilized. Some users provide DID Document that needs to be returned. This method is mainly used for verification.

获取DID历史交易只有通过Chain Resolve才可做到。

Getting DID historical transactions can only be done through Chain Resolve.

为了增加灵活性，通过resolve得到的DID Document需要用户自己保存到DID store，更新本地。

In order to increase the flexibility, the DID Document obtained through resolve needs users to save it to the DID store and update it locally.

## Chain Resolve

使用Chain Resolve方法前，确定DIDBackend已初始化，提供了resolve的链地址，否则resolve失败。

Before using the Chain Resolve method, make sure that DID Backend has been initialized and the chain address of resolve has been provided, otherwise resolve will fail.

DID在链上有三种状态：not found（链上无，即表明没有上链），valid（有效）和deactivated（失效）。

There are three states of DID on the chain: not found (not found on the chain, which means that there is no DID on the chain), valid and deactivated.

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

resolve获取链上最新（即最后一次上链）的DID Document。若链上存在，网络正常，DID Document验证有效，返回DIDDocument object；否则返回NULL，可以通过status来获取DID失败的状态。

Resolve gets the latest (that is, the last) DID Document on the chain. If it exists on the chain, the network is normal, DID Document verification is valid, and DID Document object is returned; Otherwise, NULL is returned, and the status of DID failure can be obtained by status.

`force`表示是否一定要去链上获取DID Document。SDK对于resolve结果有一定时效的缓存（默认为10分钟）。若`force`为false，且缓存在时效内，resolve返回的是缓存结果；若`force`为false但缓存生效，或者`force`为true，则直接去链上获取DID Document数据。

_force_ indicates whether it is necessary to get DID Document from the chain. There is a time-limited cache (10 minutes) for SDK resolve results. If force is false and the cache is within the time limit, resolve returns to the cache result; If force is false but the cache is in effect, or if force is true, the DID Document data will be obtained directly from the chain.

```c
DIDBiography *DID_ResolveBiography(DID *did);
```

该方法可以获取DID历史信息。若失败就返回NULL，成功返回DIDBiography object。DIDBiography内含DID状态和每次交易的内容。详细可见API文档。

This method can obtain DID history information. If it fails, NULL is returned; If it succeeds, DID Biography object is returned. DID Biography contains DID status and the content of each transaction. See detailed API documentation.

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

`DIDLocalResovleHandle`由用户提供本地DID Document接口实现，而不是从链上获取DID Document。

DIDLocalResolveHandle is implemented by the local DID Document interface provided by the user, instead of getting DID Document from the chain.

`handle`为NULL，表示不使用Local resolve方式，从链上获取DID Document。当需要使用Local resolve时，实现LocalResolveHandle类即可。

handle is NULL, which means that DID Document is obtained from the chain without Local resolve. When you need to use Local resolve, just implement the Local Resolve Handle category.
