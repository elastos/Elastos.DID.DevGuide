# Verify and use the credential

VerifiableCredential 是一个 sealed 对象，一个凭证创建后为颁发者的签名所保护，不能进行任何修改，所有的修改都会破坏其完整性。在使用凭证前需要先验证其有效性，在有效的前提下才能采信并使用凭证中包含的信息。

## 验证凭证

凭证的验证可以从下面三个方面进行验证。

### Expiration

凭证在创建的时候都有有效期，并且有效期记录在对应的 VerifiableCredential 对象中。仅有凭证在有效期内时才是一个有效的身份。对任何一个凭证，都可以通过如下的方法验证是否在有效期：

```
let serializedVc: String = ... // a serialized credential
let vc = try VerifiableCredential.fromJson(serializedVc)
let expired = try vc.isExpired()
```

### Revoked

凭证是可以被撤销的，被撤销的凭证是不能使用和被第三方接受的。可以通过如下方法测试凭证是否被撤销：

```
let serializedVc: String = ... // a serialized credential
let vc = try VerifiableCredential.fromJson(serializedVc)
let revoked = try vc.isRevoked()
```

关于如何撤销凭证参见[发布凭证](publish-credential.md)。

### Genuine

在得到一个凭证对象后，还需要测试该文档是否被恶意修改。测试方法如下：

```
let serializedVc: String = ... // a serialized credential
let vc = try VerifiableCredential.fromJson(serializedVc)
let genuine = try vc.isGenuine()
```

----

在日常使用中，验证一个凭证是否有效，需要对上述三个方面都进行验证，才能得知准确的结果。DID SDK 提供了一个复合的方法 isValid()，包含了所有验证，使用示例：

```
let serializedVc: String = ... // a serialized credential
let vc = try VerifiableCredential.fromJson(serializedVc)
let valid = try vc.isValid()
```

## 读取凭证信息

VerifiableCredential 提供了一系列的方法可以读取凭证的信息，示例：

```
let serializedVc: String = ... // a serialized credential
let vc = try VerifiableCredential.fromJson(serializedVc)

// get the issuer
let issuer = vc.issuer
// get the subject
let subject = vc.subject
// get the subject content
let name = subject.getPropertyAsString("name")
// or
let props = subject.properties()
```

 
