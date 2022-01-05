# Create and open DIDStore

## Example

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
... ... ... ... ... ... ...
store.close();
```

## Purpose

DID文档，凭证，私钥等内容都需要保存或者读取，DIDStore是整个DID体系里使用较为频繁的实例。相关操作前需要打开DIDStore，获取文件存储句柄，从而才能完成DID内容的保存或者读取。当然，用完DIDStore需将其关闭。

DID documents, credentials, private keys etc. should be saved or read, and DIDStore is a frequently used example in the whole DID system. Before any operation, users can save or read the DID content by opening the DIDStore and obtaining the file storage handle. The DIDStore should be closed after use.

## Usage

```typescript
private static CACHE_INITIAL_CAPACITY = 16;
private static CACHE_MAX_CAPACITY = 128;
    
public static async open(
    context: string,
    initialCacheCapacity: number = DIDStore.CACHE_INITIAL_CAPACITY,
    maxCacheCapacity: number = DIDStore.CACHE_MAX_CAPACITY
): Promise<DIDStore>；
```

context是需要打开的文件夹路径，若该文件夹路径不存在，SDK会自动创建。

Context is the folder path to be opened. If the folder path does not exist, SDK will automatically create it.

initialCacheCapacity是缓存初始大小，SDK默认值为16，用户也可自定义。

InitialCacheCapacity is the initial cache capacity, and the default value of SDK is 16, which can also be customized by users.

maxCacheCapacity是缓存最大容量，SDK默认值为128，用户也可自定义。

MaxCacheCapacity is the maximum cache capacity, and the default value of SDK is 128, which can also be customized by users.

```typescript
public close():void;
```

SDK会清除掉缓存等内容。

SDK will clear the chache.
