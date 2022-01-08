# DIDBackend

## Example

```typescript
let rpcEndpoint = "testnet";
let contractAddress = "0xF654c3cBBB60D7F4ac7cDA325d51E62f47ACD436";

let adapter = new Web3Adapter(rpcEndpoint, contractAddress, null, null);  
DIDBackend.initialize(adapter);
```

## Purpose

整个DID系统都需要本地与链进行交互，所以DID体系启动初期就需要先初始化DID Backend，设定好链的rpc服务端，合约地址，还有链上数据缓冲区等内容，为DID设定好与链交互接口。

The whole DID system needs to interact with the chain locally, so it is necessary to initialize the DID Backend, set the rpc server of the chain, the contract address, the data buffer on the chain at the initial stage of the DID system, thereby setting the interactive interface with the chain for DID.

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

DIDAdapter是基类，用户使用时需要自己实现基于DIDAdapter的子类，如example里面的Web3Adapter。Web3Adapter是sdk提供的DIDAdapter实现，用户可根据需求使用或者自己实现DIDAdapter。

DIDAdapter is the base class, and users need to implement their own subclasses based on DIDAdapter, such as the Web3Adapter in the example. Web3Adapter is an implementation DIDAdapterter provided by sdk. Users can use or implement DIDAdapter by themselves according to their needs.

initialCacheCapacity和maxCacheCapacity分别为缓冲的初始大小和最大容量。SDK设有默认值，用户也可根据需求自己设定。

InitialCacheCapacity and maxCacheCapacity are the initial capacity and maximum capacity of the buffer, respectively. SDK has a default value, and users can also set the value based on their own needs.

cacheTtl是缓存的有效期，默认值为10分钟。

CacheTtl is the validity period of the cache, the default value of which is 10 minutes.
