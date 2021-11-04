# Resolve DIDs

DID提供方法用于获取DID Document或者历史DID Document，并完成DID Document有效性检查。

Resolve有两种方式获取DID Document，一种是Chain Resolve，返回是链上存在的DID Document；另一种是Local Resolve，因为一些特殊情况，有时某些DID内容只需要存在但是并不需要上链，但是后续工作中又需要用到Resolve功能来获取DID Document使用，这个时候就可以用到Local Resolve。有用户提供需要返回的DID Document，这种方法主要用于验签上。

获取DID历史交易只有通过Chain Resolve才可做到。

为了增加灵活性，通过resolve得到的DID Document需要用户自己保存到DID store，更新本地。

## Chain Resolve

使用Chain Resolve方法前，确定DIDBackend已初始化，提供了resolve的链地址，否则resolve失败。

DID在链上有三种状态：not found（链上无，即表明没有上链），valid（有效）和deactivated（失效）。

### Example

```typescript
let rpcEndpoint = "testnet";
let contractAddress = "0xF654c3cBBB60D7F4ac7cDA325d51E62f47ACD436";
... ... ... ...
let adapter = new Web3Adapter(rpcEndpoint, contractAddress, null, null);  
DIDBackend.initialize(adapter);
... ... ... ...
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);

let did = new DID("did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y");
if (did) {
    //resolve did
    let doc = did.resolve(true);
    if (doc)
        console.log("resolve did {} successfully.", did);
    else
        cosole.log("resolve did {} failed.", did);
    ... ... ...
    //resolve did history
    let bio = did.resolveBiography();
    if (bio) {
        console.log("resolve did biography: count={}", bio.getTransactionCount());
        ... ... ... ...
    } else {
        console.log("resolve did biography failed.");
    }  
}
... ... ... ...
store.close();
```

### Usage

```typescript
public async resolve(
    force = false
): Promise<DIDDocument>;
```

resolve获取链上最新（即最后一次上链）的DID Document。若链上存在，网络正常，DID Document验证有效，返回DIDDocument object；否则返回null，可以通过异常来获取失败原因。

force表示是否一定要去链上获取DID Document。SDK对于resolve结果有一定时效的缓存（默认为10分钟）。若force为false，且缓存在时效内，resolve返回的是缓存结果；若force为false但缓存生效，或者force为true，则直接去链上获取DID Document数据。

```typescript
public resolveBiography(): Promise<DIDBiography>;
```

该方法可以获取DID历史信息。若失败就返回null，成功返回DIDBiography object。DIDBiography内含DID状态和每次交易的内容。详细可见API文档。

## Local Resolve

### Example

```typescript
let rpcEndpoint = "testnet";
let contractAddress = "0xF654c3cBBB60D7F4ac7cDA325d51E62f47ACD436";
... ... ... ...
let adapter = new Web3Adapter(rpcEndpoint, contractAddress, null, null);  
DIDBackend.initialize(adapter);
... ... ... ...
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);

let did = new DID("did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y");
//set resolve handle
DIDBackend.getInstance().setResolveHandle(new class implements LocalResolveHandle {
    public resolve(d: DID): DIDDocument {
        return store.loadDid(d);
    }
});

if (did) {
    // resolve doc is from line 13
    let doc = did.resolve(true);
    if (doc)
        console.log("resolve did {} successfully.", did);
    else
        cosole.log("resolve did {} failed.", did);
}
... ... ... ...
//if you don't use local chain
DIDBackend.getInstance().setResolveHandle(null);
... ... ... ...
store.close();
```

### Usage

```typescript
export interface LocalResolveHandle {
    resolve(did: DID): DIDDocument;
}

public setResolveHandle(handle: LocalResolveHandle);
```

handle为null，表示不使用Local resolve方式，从链上获取DID Document。当需要使用Local resolve时，实现LocalResolveHandle类即可。
