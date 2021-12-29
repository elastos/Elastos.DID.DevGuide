# Verify the integrity of DIDDocument

DIDDocument 是一个 sealed 对象，一旦 DID 持有人生成并签名了文档，那么文档就不能由第三方进行任何修改，所有的修改都会破坏 DIDDocument 的完整性。由此，DIDDocument 对象提供了一组验证对象的方法，可以对 DID 或者文档的如下几个方面进行测试：

DID Document is a sealed object. Once the DID controller generates and signs the document, the document cannot be modified by any third party, and all modifications will destroy the integrity of DID Document. Therefore, DID Document object provides a set of methods to verify the object, which can test the following aspects of DID or document:

* expiration
* deactivate
* genuine

## Expiration

DID 在创建的时候都有有效期，并且有效期记录在对应的 DIDDocument 中。仅有 DID 在有效期内时，该 DID 才是一个有效的身份。对任何一个 DID，都可以通过如下的方法验证是否在有效期：

A DID has an effective period when it is created, and the period is recorded in the corresponding DID Document. The DID is a valid identity only when it is within the effective period. For any DID, you can verify whether it is in the effective period by the following methods:

```java
DID did = new DID("did:elastos:icAGJstuDdRBRmx6NomZsdXLfZcAt1ANoV");
DIDDocument doc = did.resolve();
boolean expired = doc.isExpired();
```

## Deactivate

DID 是可以被撤销的，被撤销的 DID 是不能使用和被第三方接受的身份。可以通过如下方法测试 DID 是否被撤销：

The DID can be revoked, and the revoked DID is an identity that cannot be used and accepted by a third party. You can test whether DID is revoked by the following methods:

```java
DID did = new DID("did:elastos:icAGJstuDdRBRmx6NomZsdXLfZcAt1ANoV");
DIDDocument doc = did.resolve();
boolean deactivated = doc.isDeactivated();
```

## Genuine

在得到一个 DIDDocument 对象后，还需要测试该文档是否被恶意修改，这个是通过 DID 的持有人对 DID 文档本身进行签名来实现。测试方法如下：

After getting a DID Document object, it is necessary to test whether the document has been maliciously modified, which is realized by the signing of the DID document by the controller. The test method is as follows:

```java
DID did = new DID("did:elastos:icAGJstuDdRBRmx6NomZsdXLfZcAt1ANoV");
DIDDocument doc = did.resolve();
boolean genuine = doc.isGenuine();
```

***

## 全面验证

Comprehensive verification

在日常使用中，验证一个 DID 是否有效，需要对上述三个方面都进行验证，才能得知准确的结果。DID SDK 提供了一个复合的方法 isValid()，包含了所有验证，使用示例：

In daily use, in order to verify whether a DID is valid and get the accurate results, the above three aspects need to be verified. DID SDK provides a composite method isValid (), which contains all the verifications. Example:

```java
DID did = new DID("did:elastos:icAGJstuDdRBRmx6NomZsdXLfZcAt1ANoV");
DIDDocument doc = did.resolve();
boolean valid = doc.isValid();
```

> 自定义 DID 的验证方法和普通 DID 一致。
>
> The verification method of customized DID is consistent with that of primitive DID.
