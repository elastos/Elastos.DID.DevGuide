# Create or open DIDStore

在使用 DIDStore 前需要先创建或者打开 store。DIDStore 提供了 open 方法，如果 open 一个不存在的 store，那么会创建一个新的 DIDstore 并打开；如果 open 一个已经存在的 store，那么该方法只是打开这个 store。

使用 DIDStore 的示例：

```java
String storePath = "/tmp/samplestore";
DIDStore store = DIDStore.open(storePath);

// Store operations...

store.close();
```

示例中最后 close DIDStore 会销毁 DIDStore 内部的缓存对象和数据，并释放占用的系统资源。

出于效率的原因，DIDStore 内部设计有对象缓存机制，可以提供快速的 DID 对象访问，默认情况下DIDStore.open() 会使用常规大小的缓存，可以满足大多数桌面或者移动端应用的需要。如果在服务器端或者特别的需求，可以通过指定缓存大小来解决：

```java
DIDStore open(File location,
        int initialCacheCapacity,
        int maxCacheCapacity) throws DIDStoreException;
```

`initialCacheCapacity` 参数指定 store 缓存的初始大小，`maxCacheCapacity` 指定 store 缓存的最大大小。在 DIDStore 使用过程中，如果缓存用完后，将按照 LRU 策略丢弃旧缓存对象，并用新缓存对象填充。
