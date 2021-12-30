# Deactivate DID

DID 如果不再使用，可以对 DID 进行 deactivate，之后该 DID 将处于无效的状态，不能用于身份验证和其他任何 DID 相关的操作。

If the DID is out of use, it can be deactivated. The deactivated DID will be in an invalid state and cannot be used for authentication and any other DID-related operations.

DID 的 deactivate 操作通常有两种情形：

Usually, the DID can be deactivated in the following two ways:

* DID 的持有者主动的对 DID 进行 deactivate
* The controller of DID takes the initiative to deactivate the DID.
* DID 持有者遗失的该 DID 的私钥，但是该 DID 声明有委托密钥（authorizationKey），可以由被委托人代为 deactivate
* If the DID controller loses the private key to this DID, this DID can be deactivated by the trustee with authorizationKey.

通过这两种形式对 DID 进行 deactivate，其结果是相同的，都会直接导致该 DID 永久失效。

The above two methods of deactivating DID will lead to the same result, that is, the permanent invalidation of DID.

## DID 持有者 deactivate DID

The DID controller deactivates DID

DID 持有者可以以持有的该 DID 对应的默认认证密钥，并且仅能使用默认认证密钥对 DID 进行 deactivate 操作。示例如下：

The DID controller can and only can deactivate the DID with the default authentication key corresponding to the DID. The example is as follows:

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let did = try DID("did:elastos:iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2")

// Get the existing DIDDocument
let doc = try store.loadDid(did)

// Deactivate DID
doc.deactivate(storePasswd)
```

对于自定义 DID，deactivate 可以由任何一个有效的持有人（controller）对 DID 进行 deactivate，同样，deactivate 操作需要使用持有人的默认认证密钥进行签名。示例：

Any valid controller can deactivate the customized DID. Likewise, the deactivate operation should be signed by the controller with the default authentication key. Example:

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

// Deactivate ttech by iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2
try ttechDoc.deactivate(storePasswd)
```

## 被委托人代为 deactivate DID

The trustee deactivates DID

这个方式只适用于普通 DID，自定义 DID 因为有了 controller，没有设定委托人的必要。

This method is only applicable to the ordinary DID. The customized DID has its controllers, so there is no need for it to set any trustee.

普通 DID 出于安全的考虑，可以设定1个或者多个信任的委托人，体现形式就是指定信任的委托密钥。基于最小授权原则，该委托密钥仅能用于对 DID 进行 deactivate 操作。目标是在 DID 持有人遗失密钥后，可以由被委托人将 DID 失效，从而降低密钥遗失带来的安全隐患。

For security reasons, the ordinary DID can set one or more trustees by specifying the trusted key. Based on the principle of minimum authorization, this key can only be used to deactivate the DID. If the DID controller loses the key, the trustee can reduce the potential safety hazard caused by the key loss by deactivating the DID.

```
var store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
// did has a authorization key from delegatee's key #abc
let did = try DID("did:elastos:iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2")
// and the store has the private key for delegatee's key #abc
let delegatee = try DID("did:elastos:inxkMjrXt9zF1s1aoJFq1dja85C9Mm8wm6")
let keyId = try DIDURL("did:elastos:inxkMjrXt9zF1s1aoJFq1dja85C9Mm8wm6#abc")

// Get the delegatee's DIDDocument
let delegateeDoc = try store.loadDid(delegatee)

// Deactivate foobar by iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2
try delegateeDoc.deactivate(with: did, of: keyId, using: storePasswd)
```
