# Create or Open DIDStore

You need to create or open the store before using DIDStore - it provides the opening method. If you open a nonexistent store, a new DIDStore will be created and opened. If you open an existing store, then this method just opens this store.&#x20;

Please specify “YOUR-ElastosDIDSDK-INFO-PRESISTENT-PATH”:

```
let rootPath = "YOUR-ElastosDIDSDK-INFO-PRESISTENT-PATH"
let store = try DIDStore.open(atPath: rootPath)
// Store operations...
```

For efficiency, DIDStore is internally designed with an object cache mechanism, which can provide rapid access to DID objects. By default, DIDStore.open () will use a cache of regular size to meet the needs of most desktop or mobile applications. If there is a server-side or special demand, it can be solved by specifying the cache size:

```
 public class func open(atPath: String,
                           initialCacheCapacity: Int,
                           maxCacheCapacity: Int) throws -> DIDStore
```

initialCacheCapacity specifies the initial size of the store cache, and maxCacheCapacity specifies the maximum size. During the use of DIDStore, if the cache runs out, the old cache objects will be discarded and filled with new cache objects according to the LRU policy.
