# Resolve DIDs

通常使用 DID 前会将 DID 发布到 ID 侧链上，发布的 DID 都可以从 ID 侧链解析其已经发布的 DID 文档，用于对身份的验证或者其他身份相关的操作。

Usually, the DID will be published to the ID side chain before usage, and all published DID documents can be parsed from the ID side chain for identity verification or other identity-related operations.

DID SDK 提供了 DID 的解析方法，可以从 DID 标识符解析出其对应的 DIDDocument。示例如下：

DID SDK provides a method to parse DID, which can parse its corresponding DID Document from the DID identifier. Example:

```java
DID did = new DID("did:elastos:iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2");

DIDDocument doc = did.resolve();
```

解析 DID 前需要初始化 DIDBackend，其他大多是的 DID 操作也都需要初始化 DIDBackend，详见 [DIDBackend 初始化](../didbackend/)。

Before parsing DID, you need to initialize DID Backend, and most other DID operations also need to initialize DID Backend. See DID Backend initialization for details.

Java SDK 的解析实现有本地 cache 机制，类似于浏览器的 cache，用于提升 DID 解析的效率。在本地 cache 有效的情况下，resolve 会优先从本地 cache 中加载 DIDDocument。应用可以在[初始化 DIDBackend 的时候根据需求设定这些参数](../didbackend/#didbackend-cache)。

The parsing implementation of Java SDK has a local cache mechanism, like the browser’s cache, which is used to improve the efficiency of DID parsing. If the local cache is valid, resolve will load DID Document from the local cache first. The application can set these parameters according to requirements when initializing DID Backend.

如果应用需要忽略本地 cache 的结果，并且强制从 ID 侧链重新解析特定 DID，那么可以通过参数指定。例如：

If the application needs to ignore the results of the local cache and force a specific DID to be reparsed from the ID side chain, it can be specified by the parameter. Example:

```java
DID did = new DID("did:elastos:iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2");
boolean force = true;

DIDDocument doc = did.resolve(force);
```
