# Declare or revoke credential

## 公开凭证

Declare credentials

一般而言，凭证包含个人信息，从个人隐私的角度讲，应该由个人安全的持有。但是，DID 实体需要在某些场景或者某些应用中公开自己的凭证，那么 Elastos DID 提供了在 ID 侧链上发布凭证的支持。

Generally, since credentials contain personal information, credentials should be held safely by individuals from the perspective of personal privacy. However, DID entities need to declare their own credentials in some scenarios or applications. Elastos DID supports the issuance of credentials on the ID side chain.

需要注意的是，一旦凭证发布上链，那么意味着凭证包含的个人信息将永久的公开，而且不能删除或者撤回。

It should be noted that once the credential is published, the personal information contained in the credential will be permanently disclosed and cannot be deleted or withdrawn.

发布凭证需要凭证持有人使用自己的 DID 验证才能发布凭证，示例如下：

Only through authentication with his/her own DID can the credential holder publish the credential. Example:

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let vcId = try DIDURL("did:elastos:ietN9jBeA4NpSKeFMoT9GcTjpUegWHmFr2#gamescore")

let vc = try store.loadCredential(byId: vcId)
try vc.declare(storePasswd)
```

因为需要使用 DID 对发布交易进行签名，所以，在 DID store中需要有凭证持有人的 DID 文档及签名所需的私钥。

The DID is needed for signing the published transaction. Therefore, the DID store should include the DID document of the credential holder and the private key needed for signing.

## 撤销凭证

Revoke credentials

一个凭证，不论是否公开，都可以被撤销。被撤销的凭证不能再被使用，有效性验证也会失败。凭证可以由凭证的颁发者或者凭证的持有人进行撤销。

A credential, whether it is declared or not, can be revoked. The revoked credential can no longer be used, and the validity verification will fail as well. The credential can be revoked by the issuer or holder of the credential.

对凭证撤销是把被撤销的凭证标识在链上公开，但是不公开凭证的内容。如果凭证已经公开，那么也不会改变公开性。

To revoke the credential is to declare the credential identifier on the chain, but the content of the credential will not be made public. If the credential has been made public, its publicity will not be changed.

### 凭证持有人撤销凭证

The holder revokes the credential

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let vcId = try DIDURL("did:elastos:ietN9jBeA4NpSKeFMoT9GcTjpUegWHmFr2#gamescore")

let vc = try store.loadCredential(byId: vcId)
try vc.revoke(storePasswd)
```

因为需要使用 DID 对发布交易进行签名，所以，在 DID store中需要有凭证持有人的 DID 文档及签名所需的私钥。

The DID is needed for signing the published transaction. Therefore, the DID store should include the DID document of the credential holder and the private key needed for signing.

## 凭证颁发者撤销凭证

The issuer revokes the credential

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let did = try DID("did:elastos:ietN9jBeA4NpSKeFMoT9GcTjpUegWHmFr2")
// credential instance issued by ietN9jBeA4NpSKeFMoT9GcTjpUegWHmFr2 that to be revoke 
let vc: VerifiableCrdential = ...

let issuer = try store.loadDid(doc)
credential.revoke(issuer.defaultPublicKeyId(), storePasswd)
```
