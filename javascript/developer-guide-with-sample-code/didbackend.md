# DIDBackend

## Example

```typescript
let rpcEndpoint = "testnet";
let contractAddress = "0xF654c3cBBB60D7F4ac7cDA325d51E62f47ACD436";
let adapter = new Web3Adapter(rpcEndpoint, contractAddress, null, null);  
DIDBackend.initialize(adapter);
```

## Purpose

The whole DID system needs to interact with the chain locally, so it's necessary to initialize the DID Backend, set the rpc server of the chain, the contract address, the data buffer at the initial stage of the DID system, thereby setting the interactive interface with the chain for DID.

## Usage

```typescript
private static DEFAULT_CACHE_INITIAL_CAPACITY = 16;
private static DEFAULT_CACHE_MAX_CAPACITY = 64;
private static DEFAULT_CACHE_TTL = 10 * 60 * 1000;
static initialize(
    adapter: DIDAdapter,
    initialCacheCapacity: number = DIDBackend.DEFAULT_CACHE_INITIAL_CAPACITY,
    maxCacheCapacity: number = DIDBackend.DEFAULT_CACHE_MAX_CAPACITY,
    cacheTtl: number = DIDBackend.DEFAULT_CACHE_TTL
 ):void;
```

DIDAdapter is the base class, and users need to implement their own subclasses based on it (such as the Web3Adapter in the example0. Web3Adapter is an implementation DIDAdapterter provided by SDK. Users can use or implement DIDAdapter by themselves according to their needs.

InitialCacheCapacity and maxCacheCapacity are the initial and maximum capacities of the buffer, respectively. The SDK has a default value, and users can also set the value based on their own needs.

CacheTtl is the validity period of the cache, the default value of which is 10 minutes.
