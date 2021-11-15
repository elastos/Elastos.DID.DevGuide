# Access the DIDDocument

## 读取文档信息

DIDDocument 主要是用来描述 DID 的密钥信息，以及 DID 需要公开的信息。常规而言 DID 文档包含的内容分为一下类别：

- Public Keys
  - Authentication keys
  - Authorization keys
- Controllers
- Multiple-Signature
- VeriableCredentials
- Services
- Expiration date
- Document proof

不同的 DID 可能使用这些属性的一个子集。其中 VeriableCredentials 是 Elastos DID 针对 W3C DID 的扩展。原则上 DID 文档中不应该包含任何个人信息，因为 DID 文档是公开的，一旦发布以后全世界都可以读取。但是对于一些公众实体，可能希望公开特定的实体信息，那么可以把包含实体信息的可验证凭证内嵌到 DID 文档中公开。

DID 文档因为是 sealed 对象，由持有人的签名包含，所以 DIDDocument 是只读的，Java 的 DIDDocument 实现提供了一系列的方法用来读取 DID 文档中包含的信息，详细参见 //TODO: 。

## 使用 DID 对数据进行签名和验证

DID 对象除了表示身份并对身份进行验证外，也可以用于对应用数据进行签名和验证。

### DID 持有人签名数据

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret";
let did = try DID("did:elastos:idFKwBpj3Buq3XbLAFqTy8LMAW8K7kp3Ab")
var data: Data  // the data to be sign and verify

// Get the existing DIDDocument
let doc = try store.loadDid(did)
let signature = try doc.sign(using: doc, for: data)
```

持有人可以把 data 和他的 DID 生成的数据签名一起交给第三方，那么第三方就可以验证数据和签名是否匹配。

### 第三方验证数据和签名

```
let did = try DID("did:elastos:iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2") // signer‘s DID
var data: Data // the data to be verify
let signature: String // the signature signed by signer

let doc = try did.resolve()
if try doc.isValid() {
    let genuine = try doc.verify(signature: signature, onto: data)
} else {
    // Signer's DID is invalid, should report error.
}
```

