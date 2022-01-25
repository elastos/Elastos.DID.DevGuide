# Resolve DIDs

The DID provides a method of acquiring the DID document or the historical DID document, and verifying the validity of the DID document.

Resolve has two ways to obtain the DID document. The first is chain resolve, which returns the DID document that exists on the chain; the second way is local resolve. Under certain special circumstances, some DID documents only need to exist rather than to be published to the chain, but in the follow-up work, the resolve function is required to access the DID Document. At this time, local resolve can be used. The DID document that needs to be returned is provided by the user - this method is mainly used for verification.

Only through chain resolve can the historical transactions of DID be obtained.

To enhance flexibility, the DID document obtained through resolve needs to be saved to the DID store and updated locally by users themselves.

## Chain Resolve

Before using the chain resolve method, make sure that DIDBackend has been initialized and the chain address of resolve has been provided; otherwise resolve will fail.

There are three states of DID on the chain: not found (which means that the DID is not published to the chain), valid, and deactivated.

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

Resolve gets the latest DID document on the chain, or the last document published to the chain. If the document exists on the chain and the network normally works, the DID document is verified as valid, and the DIDDocument object is returned; otherwise, null is returned, and the reason of failure can be obtained by exception.

Force indicates whether it's necessary to get the DID document from the chain. The SDK has a cache freshness for the resolved results, which is 10 minutes by default. If force is false and the cache is within its validity period, resolve returns the caching results; if force is false but the cache is in effect, or if force is true, the DID document data will be obtained directly from the chain.

```typescript
public resolveBiography(): Promise<DIDBiography>;
```

This method can obtain the historical information of DID. If it fails, null is returned; if it succeeds, DIDBiography object is returned. DIDBiography contains DID status and the content of each transaction. See “API document” for details.

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

Handle is null, which means that the DID document is obtained from the chain without using local resolve. When you need to use local resolve, implement LocalResolveHandle.
