# Publish DID

DID 作为身份标识符使用的时候，通常需要先 publish 该 DID，这样才能方便的被验证方解析并验证。DID 的 publish 操作包含初始创建发布，以及更新发布。

When DID is used as an identity identifier, it is usually necessary to publish the DID first, so that it can be parsed and verified conveniently by the verified party. The publish operation of DID includes initial publication creation and publication update.

* create - 用于新创建的 DID，第一次发布到 ID 侧链
* update - 用于更新之前已经发布的 DID
* Create-used for newly created DID, which is published to ID side chain for the first time.
* Update-used to update previously published DID.

DID SDK 会自动根据 DID 的链上状态构造对应的 ID 交易。构造的 ID 交易需要使用 DID 有效的认证密钥进行签名，这意味着 DID 的持有人（controller）才能将 DID 发布上链。创建新 DID 并发布的示例：

DID SDK will automatically construct the corresponding ID transaction according to the chain state of DID. The constructed ID transaction needs to be signed with the valid authentication key of DID, which means that only the controller of DID can publish DID. Example of creating a new DID and publishing it:

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
RootIdentity identity; // a RootIdentity instance

// Get the new created DIDDocument
DIDDocument doc = identity.newDid(storePasswd);

// Publish the DID to the ID side chain using the default key
doc.publish(storePasswd);
```

发布 DID 的时候，默认是使用 DID 对应的默认认证密钥进行签名，上述示例即使用默认密钥进行交易的签名。这也是大多数应用使用的模式。如果一个 DID 设定了多个认证密钥，并且需要指定特定的认证密钥进行交易的签名，那么可以在 publish 方法中指定目标密钥的 id，并且 DIDDocument 所在的 DIDStore 中需要有该密钥对应的 PrivateKey。示例：

When publishing DID, the default is to use the default authentication key corresponding to DID to sign. In the above example, the default key is used to sign the transaction. This is also the pattern used by most applications. If a DID has set multiple authentication keys, and it is necessary to specify a specific authentication key to sign the transaction, then the ID of the target key can be specified in the publish method, and the corresponding Private Key is required in the DID Store where DID Document is located. Example:

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";

DIDDocument doc; // the DIDDocument to be publish
// The key id should defined in the document, also the private key in the store
DIDURL signKey = new DIDURL("did:elastos:igeMDX9wRHJBL72ymp3JrS3WE7jXTAs7kn#secondary");

// Publish the DID to the ID side chain using the default key
doc.publish(signKey, storePasswd);
```

更新 DID 时的发布操作和创新时的发布操作一致，DID SDK会根据链上的 DID 状态自动处理交易的类型，更新一个已经发布的 DID 文档对象并发布示例如下：

The publishing operation when updating DID is the same as that when innovating. The DID SDK will automatically process the type of transaction according to the DID status on the chain, update a published DID document object and publish an example as follows:

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID did = new DID("did:elastos:igeMDX9wRHJBL72ymp3JrS3WE7jXTAs7kn");

// Get the existing DIDDocument
DIDDocument doc = store.loadDid(did);
DIDDocument.Builder db = doc.edit();
// Do some modification on the document
DIDDocument newDoc = db.seal(storePasswd);

// Update the existing DIDDocument on the ID side chain
doc.publish(storePasswd);
```

自定义 DID 的发布和普通 DID 的发布类似，发布交易可以由任何一个持有人的有效的认证密钥、或者自身声明的认证密钥签名。示例：

The publication of customized DID is similar to that of primitive DID, and the publication transaction can be signed by any controllers’ valid authentication key or user’s own declared authentication key. Example:

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
// foobar controlled by igeMDX9wRHJBL72ymp3JrS3WE7jXTAs7kn
// and the store has the default private key for igeMDX9wRHJBL72ymp3JrS3WE7jXTAs7kn
DID did = new DID("did:elastos:igeMDX9wRHJBL72ymp3JrS3WE7jXTAs7kn");
DID foobar = new DID("did:elastos:foobar");

// Get the existing DIDDocument
DIDDocument foobarDoc = store.loadDid(foobar);
// igeMDX9wRHJBL72ymp3JrS3WE7jXTAs7kn on beharf of foobar
foobarDoc.setEffectiveController(did);

// publish foobar by igeMDX9wRHJBL72ymp3JrS3WE7jXTAs7kn
foobarDoc.publish(storePasswd);
```

上述示例中的 publish 操作类似于普通 DID 的发布，也可以指定当前 controller 的有效的认证密钥来签名交易。

The publish operation in the above example is like the publishing of primitive DID. You can also specify the valid authentication key of the current controller to sign the transaction.

> DID 文档都有有效期，有效期过期后原则上该 DID 就失效了，所以在有效期结束前需要对 DID 文档进行更新，延长有效期并发布。另外被 deactivate 的 DID 则是永久失效的 DID，不能进行任何更新和发布操作。
>
> Every DID document has an effective period. In principle, the DID will be invalid after the expiration of the effective period, so it is necessary to update the DID document, extend the effective period, and publish it before the expiration of the effective period. In addition, the deactivate DID is a permanently invalid DID and cannot be updated or published.
