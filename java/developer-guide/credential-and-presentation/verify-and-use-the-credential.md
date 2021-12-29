# How to verify and use the credential

VerifiableCredential 是一个 sealed 对象，一个凭证创建后为颁发者的签名所保护，不能进行任何修改，所有的修改都会破坏其完整性。在使用凭证前需要先验证其有效性，在有效的前提下才能采信并使用凭证中包含的信息。

Verifiable Credential is a sealed object. After a credential is created, it is protected by the issuer’s signature. It cannot be modified. All modifications will destroy its integrity. Before using the credential, the validity needs to be verified. The information contained in the credential can only be used in the premise of its validity.

## 验证凭证

Credential authentication

凭证的验证可以从下面三个方面进行验证。

The credential can be verified from the following three aspects.

### Expiration

凭证在创建的时候都有有效期，并且有效期记录在对应的 VerifiableCredential 对象中。仅有凭证在有效期内时才是一个有效的身份。对任何一个凭证，都可以通过如下的方法验证是否在有效期：

A credential has an effective period when it is created, which is recorded in the corresponding Verifiable Credential object. Only when the credential is within the effective period can it be a valid identity. For any credential, you can verify whether it is in the effective period by the following methods:

```java
String serializedVc; // a serialized credential
VerifiableCredential vc = VerifiableCredential.parse(serializedVc);
boolean expired = vc.isExpired();
```

### Revoked

凭证是可以被撤销的，被撤销的凭证是不能使用和被第三方接受的。可以通过如下方法测试凭证是否被撤销：

Credential can be revoked and the revoked one cannot be used and accepted by third parties. You can test whether the credential is revoked by the following methods: &#x20;



```java
String serializedVc; // a serialized credential
VerifiableCredential vc = VerifiableCredential.parse(serializedVc);
boolean revoked = vc.isRevoked();
```

关于如何撤销凭证参见[发布凭证](publish-credential.md)。

See publishing credential for how to revoke credential.

### Genuine

在得到一个凭证对象后，还需要测试该文档是否被恶意修改。测试方法如下：

After obtaining a credential object, it is also necessary to test whether the document has been maliciously modified. The test method is as follows:

```java
String serializedVc; // a serialized credential
VerifiableCredential vc = VerifiableCredential.parse(serializedVc);
boolean genuine = vc.isGenuine();
```

***

在日常使用中，验证一个凭证是否有效，需要对上述三个方面都进行验证，才能得知准确的结果。DID SDK 提供了一个复合的方法 isValid()，包含了所有验证，使用示例：

In daily use, to verify the validity of a credential, it is necessary to verify the above three aspects in order to know the accurate result. DID SDK provides a composite method isValid (), which contains all the verifications. Example:

```java
String serializedVc; // a serialized credential
VerifiableCredential vc = VerifiableCredential.parse(serializedVc);
boolean valid = vc.isValid();
```

## 读取凭证信息

Credential information reading

VerifiableCredential 提供了一系列的方法可以读取凭证的信息，示例：

Verifiable Credential provides a series of methods to read the information of credentials, for example:

```java
String serializedVc; // a serialized credential
VerifiableCredential vc = VerifiableCredential.parse(serializedVc);

// get the issuer
DID issuer = vc.getIssuer();
// get the subject
VerifiableCredential.Subject subject = vc.getSubject();
// get the subject content
String name = subject.getProperty("name");
// or
Map<String, Object> props = subject.getProperties();
```
