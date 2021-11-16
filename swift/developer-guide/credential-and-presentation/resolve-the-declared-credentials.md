# Resolve the declared credentials

公开的凭证都是发布到了 ID side chain 上，并且可以公开的被检索。DID SDK 提供了凭证的解析方法，可以从凭证标识符解析出其对应的凭证对象。示例如下：

```
let vcId = try DIDURL("did:elastos:imXPgH1Bx2uuUFaemJ9vwf6dVbqLWDvMiT#gamescore")

let vc = try VerifiableCredential.resolve(vcId)
```

解析凭证前需要初始化 DIDBackend，其他大多是的 DID 操作也都需要初始化 DIDBackend，详见 [DIDBackend 初始化](../didbackend/README.md)。

Swift SDK 的解析实现有本地 cache 机制，类似于浏览器的 cache，用于提升解析的效率。在本地 cache 有效的情况下，resolve 会优先从本地 cache 中加载凭证对象。应用可以在[初始化 DIDBackend 的时候根据需求设定这些参数](../didbackend/README.md#didbackend-cache)。

如果应用需要忽略本地 cache 的结果，并且强制从 ID 侧链重新解析特定凭证，那么可以通过参数指定。例如：

```
let vcId = try DIDURL("did:elastos:imXPgH1Bx2uuUFaemJ9vwf6dVbqLWDvMiT#gamescore")
let force = true

let vc = try VerifiableCredential.resolve(vcId, force)
```

