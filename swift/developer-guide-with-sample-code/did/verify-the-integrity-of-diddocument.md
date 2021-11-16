# Verify the integrity of DIDDocument

DIDDocument 是一个 sealed 对象，一旦 DID 持有人生成并签名了文档，那么文档就不能由第三方进行任何修改，所有的修改都会破坏 DIDDocument 的完整性。由此，DIDDocument 对象提供了一组验证对象的方法，可以对 DID 或者文档的如下几个方面进行测试：

- expiration
- deactivate
- genuine

## Expiration

DID 在创建的时候都有有效期，并且有效期记录在对应的 DIDDocument 中。仅有 DID 在有效期内时，该 DID 才是一个有效的身份。对任何一个 DID，都可以通过如下的方法验证是否在有效期：

```
let did = try DID("did:elastos:icAGJstuDdRBRmx6NomZsdXLfZcAt1ANoV")
let doc = try did.resolve()
let expired = doc.isExpired
```

## Deactivate

DID 是可以被撤销的，被撤销的 DID 是不能使用和被第三方接受的身份。可以通过如下方法测试 DID 是否被撤销：

```
let did = try DID("did:elastos:icAGJstuDdRBRmx6NomZsdXLfZcAt1ANoV")
let doc = try did.resolve()
let deactivated = doc.isDeactivated
```

## Genuine

在得到一个 DIDDocument 对象后，还需要测试该文档是否被恶意修改，这个是通过 DID 的持有人对 DID 文档本身进行签名来实现。测试方法如下：

```
let did = try DID("did:elastos:icAGJstuDdRBRmx6NomZsdXLfZcAt1ANoV")
let doc = try did.resolve()
let genuine = doc.isGenuine()
```

## 全面验证

在日常使用中，验证一个 DID 是否有效，需要对上述三个方面都进行验证，才能得知准确的结果。DID SDK 提供了一个复合的方法 isValid()，包含了所有验证，使用示例：

```
let did = try DID("did:elastos:icAGJstuDdRBRmx6NomZsdXLfZcAt1ANoV")
let doc = try did.resolve()
let valid = try doc.isValid()
```

> 自定义 DID 的验证方法和普通 DID 一致。


