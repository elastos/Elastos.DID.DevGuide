# Resolve DIDs

Usually, the DID will be published to the ID sidechain before usage, and all published DID documents can be parsed from the ID sidechain for identity verification or other identity-related operations.

The DID SDK provides a method to parse DID, which can parse its corresponding DID Document from the DID identifier. For example:

```java
DID did = new DID("did:elastos:iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2");

DIDDocument doc = did.resolve();
```

Before parsing the DID, you need to initialize the DID Backend - most other DID operations also need to initialize the DID Backend. See [DID Backend initialization](../didbackend/) for details.

The parsing implementation of Java SDK has a local cache mechanism, like the browser’s cache, which is used to improve the efficiency of DID parsing. If the local cache is valid, resolve will load DID Document from the local cache first. The application can set these parameters according to requirements when initializing [DID Backend](../didbackend/#didbackend-cache).

If the application needs to ignore the results of the local cache and force a specific DID to be reparsed from the ID sidechain, it can be specified by the parameter. Here's an example:

```java
DID did = new DID("did:elastos:iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2");
boolean force = true;

DIDDocument doc = did.resolve(force);
```
