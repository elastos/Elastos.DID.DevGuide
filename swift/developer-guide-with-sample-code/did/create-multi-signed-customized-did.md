# Create multi-signed customized DID

创建多签的自定义 DID 前，需要 DID 的多个 controller 达成共识，并确定多签的规则，并据此规则完成 DID 的创建流程。

自定义 DID 的多签规则的表达是 `m:n`，即n签m（m of n）。含义是 DID 由 n 个 controllers 持有，任何变更 DID 的操作都需要其中 m 个 controllers 的签名才能生效。

 多签 DID 的创建需要由 n 个 controllers 中的一个 controller 的 DID 发起自定义 DID 的创建，并按照 DIDDocument 定义的多签规则，完成后续的签名的补充，直到达到 m 个有效签名。这是 DIDDocument 能成为有效的文档，并可以发布上链。

这里的示例是一个创建3签2的多签 DID 。假定已经存在了 3 个 controllers，DID 分别如下：
- Alice - did:elastos:iXkgy7RruwjJDP2o3pfPaxpVR65bEzzjtz
- Bob - did:elastos:iUCXpvpNZ9B9vwWf6PMFMJ6duRPPiLSWSw
- Carol - did:elastos:ignLDwpCumFLA2GrXptr5zNugtMcfvoM4d

他们要创建 2:3 的多签 DID `did:elastos:example`。

## 1. Alice 先创建初始的文档

```
var store: DIDStore // an opened DIDStore instance
let storePasswd = "secret"
let alice = try DID("did:elastos:iXkgy7RruwjJDP2o3pfPaxpVR65bEzzjtz")
let bob = try DID("did:elastos:iUCXpvpNZ9B9vwWf6PMFMJ6duRPPiLSWSw")
let carol = try DID("did:elastos:ignLDwpCumFLA2GrXptr5zNugtMcfvoM4d")
let example = try DID("did:elastos:example")

// Alice get her document
let aliceDoc = try store.loadDid(alice)

// Create example's DID
let exampleDoc = try aliceDoc.newCustomizedDid(withId: example, [alice, bob, carol], 2, storePassword)

// Serialize the document to the string
let serializedDoc = exampleDoc.toString()
```
Alice 创建了 example 的自定义 DID，controllers 是 Alice，Bob 和 Carol，同时指定了需要 2 个签名，即3签2。但是这个 DIDDocument 实例还不是一个有效的 DID 文档，因为目前仅仅包含了 Alice 的签名，还没有达到 2 个签名的要求。所以 Alice 需要将这个文档序列化，并通过应用提供的途径交给 Bob 或者 Carol 对 DIDDocument 继续进行签名。本示例假定由 Bob 进行签名。

## 2. Bob 对文档进行签名

```
var store:DIDStore // an opened DIDStore instance
let storePasswd = "secret"
let bob = try DID("did:elastos:iUCXpvpNZ9B9vwWf6PMFMJ6duRPPiLSWSw")
let serializedDoc // the serialized document got from Alice

// Bob get his document
let bobDoc = try store.loadDid(bob)
let exampleDoc = try DIDDocument.convertToDIDDocument(fromJson: serializedDoc)

// Bob sign the example's DID document
let signedExampleDoc = bobDoc.sign(with: exampleDoc, using: storePasswd)

// Bob can publish the example's DID
try signedExampleDoc.setEffectiveController(bob)
try signedExampleDoc.publish(storePasswd)
```

## 3. 使用自定义 DID

在发布好 example 的 DID 后，Alice、Bob 和 Carol 都可以从链上获取 example 的 DID 文档，并用自己的 DID 作为有效的 controller，使用 example 的身份。但仅限于使用 DID 身份，不能对 example 的 DID 文档进行变更。如果需要变更，那么同样需要按照上述流程完成多签。

对 DID 文档的变更不应该涉及到并更 controller 或者多签规则，如果需要修个这两项内容，属于 DID 所有权的变更，变更流程参照//TODO:。


参数：

* did: the new customized identifier
* controllers: the other controllers for the new customized DID
* multisig: how many signatures are required
* storePassword: the password for the DIDStore
