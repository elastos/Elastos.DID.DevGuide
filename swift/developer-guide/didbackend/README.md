# DIDBackend

DIDBackend object is the abstraction of the EID sidechain of the DID SDK at the application client. The DID SDK interacts with EID sidechain through DIDBackend object and realizes the operations of DID parsing, publishing, and updating, as well as the parsing and publishing of verifiable credentials. When using the DID SDK, the application needs to first initialize the global DIDBackend object.

During initialization, DIDBackend needs to provide methods of how to publish DID transactions and how to parse DID objects. Developers can implement these two methods according to the requirements and context of the application, and provide them for DIDBackend through the DIDAdapter interface. The DID SDK realizes the implementation of DefaultDIDAdapter which will use elastos.io or trinity-tech.io services to parse DID and achieve an empty implementation to publish DID transactions. DefaultDIDAdapter satisfies the applications’ demands, such as reading and verifying DID.

For instance, DefaultDIDAdapter is used to initialize read only DIDBackend objects:

```
try DIDBackend.initialize(DefaultDIDAdapter(resolver: "mainnet"))
```

DefaultDIDAdapter also accepts mainnet and testnet parameters, which correspond to Elastos’ main and test networks, respectively. It can also be the URL of DID resolver service provided by an application developer.

## Implementation of DIDAdapter

If the application needs to publish the DID object besides parsing and verifying the DID, then the it should provide a method to create and publish the DID transaction. DIDBackend can be initialized by implementing DIDAdapter interface and applying the implemented DIDAdapter. Examples are as follows:

```
class DIDExampleAdapter: DefaultDIDAdapter {
    override func createIdTransaction(_ payload: String, _ memo: String?) throws {
       //  实现
       ...
    }
}

let adapter = DIDExampleAdapter(resolver: "mainnet") 
try DIDBackend.initialize(adapter)
```

Applications can create and publish DID transactions based on their own needs. There are mainly three ways:

* Call Elastos Essentials through intent and it will create and publish the transaction
* Create and publish transactions using your own Elastos wallet
* Create and publish transactions through open DID API services

The sample code in the [DID SDK GitHub repository](https://github.com/elastos/Elastos.DID.Swift.SDK/blob/master/ElastosDIDSDKTests/Web3Adapter.swift) includes a reference implementation based on Web3 and a reference implementation based on the [Assist API of Tuum Tech](https://github.com/elastos/Elastos.DID.Swift.SDK/blob/feat\_demo/DIDExample/DIDExample/AssistDIDAdapter.swift).

## Initialization and Local Cache of DIDBackend <a href="#didbackend-cache" id="didbackend-cache"></a>

The DID parse is an operation widely used by the DID SDK and applications. In order to optimize the efficiency of DID parsing, the DIDBackend implementation of Swift SDK internally uses a DID parsing cache to cache the parsed DID, and accelerates the DID parsing using local cache objects during their validity period. By default, DIDBackend will use the cache of the default size and TTL of the cached object. The default setting can meet the needs of most desktop or mobile applications. If there is a special demand on the server side, it can be realized by specifying the cache size or TTL.

```
class DIDExampleAdapter: DefaultDIDAdapter {
    override func createIdTransaction(_ payload: String, _ memo: String?) throws {
       //  实现
       ...
    }
}

let adapter = DIDExampleAdapter(resolver: "mainnet") 
let initialCacheCapacity = 32
let maxCacheCapacity = 128
let ttl = 30 * 60 * 1000; // 30 minutes

try DIDBackend.initialize(adapter, initialCacheCapacity, maxCacheCapacity, ttl)
```
