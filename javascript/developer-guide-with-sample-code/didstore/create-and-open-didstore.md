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

initialCacheCapacity是缓存初始大小，SDK默认值为16，用户也可自定义。

maxCacheCapacity是缓存最大容量，SDK默认值为128，用户也可自定义。

```typescript
public close():void;
```

SDK会清除掉缓存等内容。
