# DIDBackend

DIDBackend 对象是 DID SDK 在应用端的 EID side chain 的抽象。DID SDK 通过 DIDBackend 对象和 EID side chain 进行交互，并实现 DID 的解析、发布、更新等操作，以及可验证凭证的解析和发布。应用在使用 DID SDK 时，需要首先初始化全局的 DIDBackend 对象。

DIDBackend 在初始化的时候需要提供一个如何发布 DID 交易和如何解析 DID 对象的方法。开发者可以根据应用的需求和上下文实现这两个方法，并在通过 DIDAdapter 接口提供给 DIDBackend。DID SDK 实现了一个 DefaultDIDAdapter 的实现，DefaultDIDAdapter 将使用 elastos.io 或者 trinity-tech.io 的服务实现 DID 的解析，以及一个发布 DID 交易的空实现。DefaultDIDAdapter 可以满足应用读取和验证 DID 等相关的需求。

例如，使用 DefaultDIDAdapter 来初始化只读的 DIDBackend 对象：

```java
DIDBackend.initialize(new DefaultDIDAdapter("mainnet"));
```

DefaultDIDAdapter 可以接受 `mainnet` 和 `testnet` 参数，分别对应 Elastos 的主网和测试网；也可以是一个应用开发者提供的 DID resolver 服务的 URL。

## 实现 DIDAdapter

如果应用除了解析、验证 DID 等操作外，还需要发布 DID 对象，那么需要由应用提供一个创建发布 DID 交易的方法。可以通过实现DIDAdapter接口，并通过应用实现的 DIDAdapter 来初始化 DIDBackend。示例如下：

```java
DIDBackend.initialize(new DefaultDIDAdapter("mainnet") {
    @Override
    public void createIdTransaction(String payload, String memo)
            throws DIDTransactionException {
        // Application defined method to publish the DID transaction
        // This sample just print the DID transaction payload to console
        System.out.println(payload);
    }
});
```

应用可以根据自身的需要来实现 DID 交易的创建和发布。主要的方式有三种：

- 通过 intent 呼叫 Elastos Essentials，由 Essentials 完成交易的创建和发布交易
- 通过应用自己的 Elastos 钱包创建和发布交易
- 通过公开的 DID API 服务创建和发布交易

在 DID SDK git 仓库的示例代码中包含了一个[基于 Web3 的参考实现](https://github.com/elastos/Elastos.DID.Java.SDK/blob/master/samples/src/main/java/org/elastos/did/samples/Web3Adapter.java)，和一个[基于 Tuum Tech 的 Assist API 的参考实现](https://github.com/elastos/Elastos.DID.Java.SDK/blob/master/samples/src/main/java/org/elastos/did/samples/AssistDIDAdapter.java)。

## DIDBackend 的初始化和本地缓存 {#didbackend-cache}

DID 解析操作是 DID SDK 和应用大量使用的一个操作，Java SDK 的 DIDBackend 实现为了优化 DID 解析的效率，内部使用了一个DID 解析缓存，用来缓存已经解析的 DID，并在缓存对象的有效期内，使用本地的缓存对象加速 DID 的解析。默认情况下 DIDBackend 会使用默认大小的缓存，以及缓存对象的 TTL，默认设定可以满足大多数桌面或者移动端应用的需要。如果在服务器端或者特别的需求，可以通过指定缓存大小或者 TTL 来实现。

```java
DIDAdapter adapter = new DefaultDIDAdapter("mainnet") {
    @Override
    public void createIdTransaction(String payload, String memo)
            throws DIDTransactionException {
        // Application defined method to publish the DID transaction
        // This sample just print the DID transaction payload to console
        System.out.println(payload);
    }
};
int initialCacheCapacity = 32;
int maxCacheCapacity = 128;
int ttl = 30 * 60 * 1000; // 30 minutes

DIDBackend.initialize(adapter, initialCacheCapacity, maxCacheCapacity, ttl);
```

