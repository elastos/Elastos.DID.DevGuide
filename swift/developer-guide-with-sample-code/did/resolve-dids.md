---
description: 解析DID，通过DID Resolve返回相应的DIDDocument, 解析DID有同步和异步2种方法，可以根据项目需要选择使用不同的方法。
---

# Resolve DIDs

同步例子：

```
let did = try DID("did:elastos:idFKwBpj3Buq3XbLAFqTy8LMAW8K7kp3Ab")
let document = try did.resolve()
```

异步例子：

```
let did = try DID("did:elastos:idFKwBpj3Buq3XbLAFqTy8LMAW8K7kp3Ab")
_ = did.resolveAsync().done { document in
   ... // 拿到了Document
   }.catch { error in
   ... // 遇到了error
 }
```
