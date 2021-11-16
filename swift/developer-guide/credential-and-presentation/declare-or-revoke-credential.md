# Declare or revoke credential

## 公开凭证

一般而言，凭证包含个人信息，从个人隐私的角度讲，应该由个人安全的持有。但是，DID 实体需要在某些场景或者某些应用中公开自己的凭证，那么 Elastos DID 提供了在 ID 侧链上发布凭证的支持。

需要注意的是，一旦凭证发布上链，那么意味着凭证包含的个人信息将永久的公开，而且不能删除或者撤回。

发布凭证需要凭证持有人使用自己的 DID 验证才能发布凭证，示例如下：

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let vcId = try DIDURL("did:elastos:ietN9jBeA4NpSKeFMoT9GcTjpUegWHmFr2#gamescore")

let vc = try store.loadCredential(byId: vcId)
try vc.declare(storePasswd)
```

因为需要使用 DID 对发布交易进行签名，所以，在 DID store中需要有凭证持有人的 DID 文档及签名所需的私钥。

## 撤销凭证

一个凭证，不论是否公开，都可以被撤销。被撤销的凭证不能再被使用，有效性验证也会失败。凭证可以由凭证的颁发者或者凭证的持有人进行撤销。

对凭证撤销是把被撤销的凭证标识在链上公开，但是不公开凭证的内容。如果凭证已经公开，那么也不会改变公开性。

### 凭证持有人撤销凭证

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let vcId = try DIDURL("did:elastos:ietN9jBeA4NpSKeFMoT9GcTjpUegWHmFr2#gamescore")

let vc = try store.loadCredential(byId: vcId)
try vc.revoke(storePasswd)
```

因为需要使用 DID 对发布交易进行签名，所以，在 DID store中需要有凭证持有人的 DID 文档及签名所需的私钥。

## 凭证颁发者撤销凭证

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let did = try DID("did:elastos:ietN9jBeA4NpSKeFMoT9GcTjpUegWHmFr2")
// credential instance issued by ietN9jBeA4NpSKeFMoT9GcTjpUegWHmFr2 that to be revoke 
let vc: VerifiableCrdential = ...

let issuer = try store.loadDid(doc)
credential.revoke(issuer.defaultPublicKeyId(), storePasswd)
```

