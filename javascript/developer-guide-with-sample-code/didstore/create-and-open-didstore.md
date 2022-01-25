# Create and Open DIDStore

## Example

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
... ... ... ... ... ... ...
store.close();
```

## Purpose

DID documents, credentials, private keys, etc. should be saved or read, and DIDStore is a frequently used example in the whole DID system. Before any operation, users can save or read the DID content by opening the DIDStore and obtaining the file storage handle. The DIDStore should be closed after use.

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

Context is the folder path to be opened. If the folder path does not exist, the SDK will automatically create it.

InitialCacheCapacity is the initial cache capacity, and the default value of the SDK is 16, which can also be customized by users.

MaxCacheCapacity is the maximum cache capacity, and the default value of the SDK is 128, which can also be customized by users.

```typescript
public close():void;
```

SDK will clear the chache.
