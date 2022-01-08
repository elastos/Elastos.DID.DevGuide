# Resolve DIDs

Usually, the DID will be published to the ID sidechain before it's used, and all the published DIDs can parse its DID documents from the ID sidechain for identity verification or other identity-related operations.

The DID SDK provides a method to parse DIDs, and the corresponding DID documents can be parsed from the DID identifier. Examples are as follows:

```
let did = try DID("did:elastos:iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2")
let doc = try did.resolve()
```

DIDBackend should be initialized before parsing the DID, which also applies to the other DID operations in the majority. For details, see [Initialization and local cache of DIDBackend](../didbackend/).

The implementation of SDK parsing has a local cache mechanism, similar to the browser’s cache, which improves the efficiency of DID parsing. If the local cache is valid, resolve will load DIDDocument from the local cache first. Applications can set these parameters per their demands when [initializing DIDBackend](../didbackend/#didbackend-cache).

If the application needs to ignore the results of the local cache and force a specific DID to be reparsed from the ID sidechain, it can be specified by the parameter. As an example:

```
let did = try DID("did:elastos:iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2")
let force = true
let doc = try did.resolve(force)
```
