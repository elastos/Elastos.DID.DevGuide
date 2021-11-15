# Resolve DIDs

DID提供方法用于获取DID Document或者历史DID Document，并完成DID Document有效性检查。

Resolve有两种方式获取DID Document，一种是Chain Resolve，返回是链上存在的DID Document；另一种是Local Resolve，因为一些特殊情况，有时某些DID内容只需要存在但是并不需要上链，但是后续工作中又需要用到Resolve功能来获取DID Document使用，这个时候就可以用到Local Resolve。有用户提供需要返回的DID Document，这种方法主要用于验签上。

获取DID历史交易只有通过Chain Resolve才可做到。

为了增加灵活性，通过resolve得到的DID Document需要用户自己保存到DID store，更新本地。

## Chain Resolve

使用Chain Resolve方法前，确定DIDBackend已初始化，提供了resolve的链地址，否则resolve失败。

DID在链上有三种状态：not found（链上无，即表明没有上链），valid（有效）和deactivated（失效）。

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

`force`表示是否一定要去链上获取DID Document。SDK对于resolve结果有一定时效的缓存（默认为10分钟）。若`force`为false，且缓存在时效内，resolve返回的是缓存结果；若`force`为false但缓存生效，或者`force`为true，则直接去链上获取DID Document数据。

```c
DIDBiography *DID_ResolveBiography(DID *did);
```

该方法可以获取DID历史信息。若失败就返回NULL，成功返回DIDBiography object。DIDBiography内含DID状态和每次交易的内容。详细可见API文档。

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

`handle`为NULL，表示不使用Local resolve方式，从链上获取DID Document。当需要使用Local resolve时，实现LocalResolveHandle类即可。

