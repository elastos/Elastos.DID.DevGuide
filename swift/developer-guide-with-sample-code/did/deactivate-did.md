---
description: Deactivate DID 分为同步和异步2种方法，可根据自己情况使用。
---

# Deactivate DID

普通Document 停用：

```
// *** 创建之后就停用 ***
var doc = try identity.newDid("YOUR-STORE-PASSWORD")
try doc.publish(using: "YOUR-STORE-PASSWORD")
try doc.deactivate(using: "YOUR-STORE-PASSWORD")
// 验证， 停用成功返回true，反之，false
let resulet = doc.isDeactivated
```

自定义Document 停用：

```
// *** 创建之后就停用 ***
// Create normal DID first
let controller = try identity.newDid("YOUR-STORE-PASSWORD")
try controller.publish(using: "YOUR-STORE-PASSWORD")

// Create customized DID
let did = try DID("did:elastos:helloworld")
var doc = try controller.newCustomizedDid(withId: did, "YOUR-STORE-PASSWORD")
try doc.publish(using: "YOUR-STORE-PASSWORD")

// Deactivate
try doc.deactivate(using: "YOUR-STORE-PASSWORD")
```

多签文档停用：

```
// *** 创建之后就停用 ***
// Create normal DID first
let ctrl1 = try identity.newDid("YOUR-STORE-PASSWORD")
try ctrl1.publish(using: "YOUR-STORE-PASSWORD")

let ctrl2 = try identity.newDid("YOUR-STORE-PASSWORD")
try ctrl2.publish(using: "YOUR-STORE-PASSWORD")

let ctrl3 = try identity.newDid("YOUR-STORE-PASSWORD")
try ctrl3.publish(using: "YOUR-STORE-PASSWORD")

// Create customized DID
let did = try DID("did:elastos:helloworld3")
var doc = try ctrl1.newCustomizedDid(withId: did, [ctrl2.subject, ctrl3.subject], 2, "YOUR-STORE-PASSWORD")
doc = try ctrl2.sign(with: doc, using: "YOUR-STORE-PASSWORD")
try doc.setEffectiveController(ctrl1.subject)
try doc.publish(using: "YOUR-STORE-PASSWORD")

// Deactivate
try doc.deactivate(with: ctrl1.defaultPublicKeyId()!, using: "YOUR-STORE-PASSWORD")
```

参数：Deactivate this DID using the authentication key.

* signKey: the key to sign the transaction
* storePassword: the password for the DIDStore
* adapter: an optional DIDTransactionAdapter, if null the method will use the default adapter from the DIDBackend
