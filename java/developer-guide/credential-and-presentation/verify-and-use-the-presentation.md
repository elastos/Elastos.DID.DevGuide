# How to verify and use the presentation

VerifiablePresentation 是一个 sealed 对象，创建后为创建者的签名所保护，不能进行任何修改，所有的修改都会破坏其完整性。在使用 VerifiablePresentation 前需要先验证其有效性，以及包含的凭证的有效性，在有效的前提下才能采信并使用凭证中包含的信息。

Verifiable Presentation is a sealed object, which is protected by the creator’s signature after creation, and can’t be modified at all. All modifications will destroy its integrity. Before using the Verifiable Presentation, it is necessary to verify its validity and the validity of the credential contained therein, so that the information contained in the credential can be accepted and used only if it is valid.

## 验证 presentation

Verify presentation

对 presentation 的验证需要包含对 presentation 本身的签名，以及内部包含的所有凭证进行验证。VerifiablePresentation 的 isValid() 方法，包含了对所有上述项目的验证，使用示例：

The verification of the presentation needs to include the signature of the presentation itself, as well as the verification of all internal credentials. The isValid () method of Verifiable Presentation includes the verification of all the above items. Example:

```java
String serializedVp; // a serialized presentation
VerifiablePresentation vp = VerifiablePresentation.parse(serializedVp);
boolean valid = vp.isValid();
```

## 读取 VerifiablePresentation 的信息

Information reading of Verifiable Presentation

VerifiablePresentation 提供了方法可以读取所包含的凭证，以及验证信息，示例：

Verifiable Presentation provides a method to read the contained credentials and verify the information. Example:

```java
String serializedVp; // a serialized presentation
VerifiablePresentation vp = VerifiablePresentation.parse(serializedVp);

// get the holder
DID holder = vp.getHolder();
// get the embedded credentials
List<VerifiableCredential> vcs = vp.getCredentials();
// get the Proof
VerifiablePresentation.Proof proof = vp.getProof();
String realm = proof.getRealm();
String nonce = proof.getNonce();
```
