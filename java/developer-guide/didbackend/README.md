# DID Backend

The DID Backend object is the abstraction of the EID sidechain of DID SDK on the application side. The DID SDK interacts with the EID sidechain through the DID Backend object, and realizes the operations of DID parsing, publishing, and updating, as well as the parsing and publishing of verifiable credentials. When using the DID SDK, the application needs to initialize the global DID Backend object first.

The DID Backend needs to provide a method of how to publish DID transactions and parse DID objects during initialization. Developers can implement these two methods according to the requirements and context of the application, then providing them to the DID Backend through the DID Adapter interface. The DID SDK accomplishes the implementation of Default DID Adapter, which will utilize elastos.io or trinity-tech.io services to parse DID, and an empty implementation to publish DID transactions. The Default DID Adapter can meet the needs of applications such as reading and verifying the DID.&#x20;

For example, a read-only DID Backend object can be initialized through using Default DID Adapter:

```java
DIDBackend.initialize(new DefaultDIDAdapter("mainnet"));
```

The Default DID Adapter can accept mainnet and testnet parameters, which correspond to Elastos’ main and test networks, respectively. It can also be the URL of the DID resolver service provided by an application developer.

## Implement DID Adapter

If the application needs to publish the DID object besides parsing and verifying the DID, it needs to provide a method to create and publish the DID transaction. You can initialize the DID Backend by implementing the DID Adapter interface and applying the implemented DID Adapter. Examples are as follows:

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

Applications can create and publish DID transactions according to their own needs. There are three main ways:

* Call Elastos Essentials through intent, and Essentials will complete the creation and publication of the transaction.
* Create and publish transactions by applying your own Elastos wallet
* Create and publish transactions through the exposed DID API service

The example code of the DID SDK git warehouse includes a reference implementation based on Web3 and another based on the Assist API of Tuum Tech.

## Initialization and Local Cache of DID Backend

The DID parsing operation is widely used by the DID SDK and applications. In order to optimize the efficiency of DID parsing, the DID Backend implementation of Java SDK internally uses a DID parsing cache to cache the parsed DID, and uses local cache objects to speed up the DID parsing during the effective period of the cached objects. By default, the DID Backend will use the cache of the default size and TTL of the cached object. The default setting can meet the needs of most desktop or mobile applications. If there is a special demand on the server side, it can be realized by specifying the cache size or TTL.

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
