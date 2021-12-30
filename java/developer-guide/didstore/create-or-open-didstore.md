# Create or open DIDStore

在使用 DIDStore 前需要先创建或者打开 store。DIDStore 提供了 open 方法，如果 open 一个不存在的 store，那么会创建一个新的 DIDstore 并打开；如果 open 一个已经存在的 store，那么该方法只是打开这个 store。

A store needs to be created or opened before using DID Store. DID Store provides the open method. If a nonexistent store is opened, a new DID store will be created and opened. If an existing store is opened, then this method is just for opening.

使用 DIDStore 的示例：

Example of using DID Store:

```java
String storePath = "/tmp/samplestore";
DIDStore store = DIDStore.open(storePath);

// Store operations...

store.close();
```

示例中最后 close DIDStore 会销毁 DIDStore 内部的缓存对象和数据，并释放占用的系统资源。

At the end of the example, close DID Store will destroy the cached objects and data inside DID Store and release the occupied system resources.

出于效率的原因，DIDStore 内部设计有对象缓存机制，可以提供快速的 DID 对象访问，默认情况下DIDStore.open() 会使用常规大小的缓存，可以满足大多数桌面或者移动端应用的需要。如果在服务器端或者特别的需求，可以通过指定缓存大小来解决：

For efficiency reasons, DID Store is internally designed with an object cache mechanism, which can provide fast DID object access. By default, DID Store.open () will use a cache of regular size, which can meet the needs of most desktop or mobile applications. If there is a server-side or special demand, it can be solved by specifying the cache size:

```java
DIDStore open(File location,
        int initialCacheCapacity,
        int maxCacheCapacity) throws DIDStoreException;
```

`initialCacheCapacity` 参数指定 store 缓存的初始大小，`maxCacheCapacity` 指定 store 缓存的最大大小。在 DIDStore 使用过程中，如果缓存用完后，将按照 LRU 策略丢弃旧缓存对象，并用新缓存对象填充。

The initial Cache Capacity parameter specifies the initial size of the store cache, and max Cache Capacity specifies the maximum size of the store cache. During the use of DID Store, if the cache runs out, the old cache objects will be discarded according to LRU policy and filled with new cache objects.
