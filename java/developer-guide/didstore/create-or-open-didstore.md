# Create or Open DID Store

A store needs to be created or opened before using DID Store, as it provides the open method. If a nonexistent store is opened, a new DID store will be created and opened. If an existing store is opened, then this method is just for opening.

Example of using DID Store:

```java
String storePath = "/tmp/samplestore";
DIDStore store = DIDStore.open(storePath);

// Store operations...

store.close();
```

At the end of the example, close DID Store will destroy the cached objects and data inside it and release the occupied system resources.

For efficiency reasons, DID Store is internally designed with an object cache mechanism, which can provide fast DID object access. By default, DID Store.open () will use a cache of regular size, which can meet the needs of most desktop or mobile applications. If there is a server-side or special demand, it can be solved by specifying the cache size:

```java
DIDStore open(File location,
        int initialCacheCapacity,
        int maxCacheCapacity) throws DIDStoreException;
```

The initialCacheCapacity parameter specifies the initial size of the store cache, and maxCacheCapacity specifies the maximum size of the store cache. During the use of DID Store, if the cache runs out, the old cache objects will be discarded according to LRU policy and filled with new cache objects.
