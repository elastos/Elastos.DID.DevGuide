# Publish DID

DID 作为身份标识符使用的时候，通常需要先 publish 该 DID，这样才能方便的被验证方解析并验证。DID 的 publish 操作包含初始创建发布，以及更新发布。

- create - 用于新创建的 DID，第一次发布到 ID 侧链
- update - 用于更新之前已经发布的 DID


DID SDK 会自动根据 DID 的链上状态构造对应的 ID 交易。构造的 ID 交易需要使用 DID 有效的认证密钥进行签名，这意味着 DID 的持有人（controller）才能将 DID 发布上链。创建新 DID 并发布的示例：


```
var store: DIDStore // an opened DIDStore instance
let storePasswd = "secret"
let identity: RootIdentity = ... // a RootIdentity instance

// Get the new created DIDDocument
let doc = try identity.newDid(storePasswd)

// Publish the DID to the ID side chain using the default key
try doc.publish(using: storePasswd)
```

发布 DID 的时候，默认是使用 DID 对应的默认认证密钥进行签名，上述示例即使用默认密钥进行交易的签名。这也是大多数应用使用的模式。如果一个 DID 设定了多个认证密钥，并且需要指定特定的认证密钥进行交易的签名，那么可以在 publish 方法中指定目标密钥的 id，并且 DIDDocument 所在的 DIDStore 中需要有该密钥对应的 PrivateKey。示例：

```
var store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"

let doc: DIDDocument = ... // the DIDDocument to be publish
// The key id should defined in the document, also the private key in the store
let signKey = try DIDURL("did:elastos:iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2#secondary")

// Publish the DID to the ID side chain using the default key
try doc.publish(with: signKey, using: storePasswd)
```

更新 DID 时的发布操作和创新时的发布操作一致，DID SDK会根据链上的 DID 状态自动处理交易的类型，更新一个已经发布的 DID 文档对象并发布示例如下：

```
var store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let did = try DID("did:elastos:iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2")

// Get the existing DIDDocument
let doc = try store.loadDid(did)
let db = try doc.editing()
// Do some modification on the document
let newDoc = db.seal(using: storePasswd)

// Update the existing DIDDocument on the ID side chain
try doc.publish(using:storePasswd)
```

自定义 DID 的发布和普通 DID 的发布类似，发布交易可以由任何一个持有人的有效的认证密钥、或者自身声明的认证密钥签名。示例：

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
// ttech controlled by iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2
// and the store has the default private key for iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2
let did = try DID("did:elastos:iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2")
let ttech = try DID("did:elastos:ttech")

// Get the existing DIDDocument
let ttechDoc = try store.loadDid(ttech)
// iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2 on beharf of ttech
try ttechDoc.setEffectiveController(did)

// publish ttech by iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2
ttechDoc.publish(using: storePasswd)
```

上述示例中的 publish 操作类似于普通 DID 的发布，也可以指定当前 controller 的有效的认证密钥来签名交易。

转让document并发布到链上

- 首先创建TransferTicket
- 参数did： 为需要转让的DID
- to：接受转让的did（转让个给谁）
- storePassword：本地私钥加密密码

```
// create new controller
let newController = try identity.newDid(storePassword)
let ticket = try controller.createTransferTicket(withId: did, to: newController.subject, using: storePassword)
// create new document for customized DID
let newDoccument = try newController.newCustomizedDid(withId: did, true, storePassword)
```

* > DID 文档都有有效期，有效期过期后原则上该 DID 就失效了，所以在有效期结束前需要对 DID 文档进行更新，延长有效期并发布。另外被 deactivate 的 DID 则是永久失效的 DID，不能进行任何更新和发布操作。




