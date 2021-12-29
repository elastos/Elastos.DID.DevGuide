# Package the crdentials as presentation for the selective disclosure request

出于安全的考虑，凭证不能直接交给由持有人交给验证方，作为个人信息凭证。需要提交给验证方的凭证需要打包为 VerifiablePresentation，在 VerifiablePresentation 中会包含凭证的用途，验证会话标识等信息，以及有效凭证持有人的签名。

For security reasons, the credential cannot be handed over directly as personal information credential to the verifier by the controller. The credential to be submitted to the verifier needs to be packaged as a Verifiable Presentation, which will include the purpose of the credential, the identification of the verification session and other information, as well as the signature of the valid credential controller.

将凭证打包为 VerifiablePresentation 的示例如下：

Example of packaging credential as Verifiable Presentation is as follows:

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID did = new DID("did:elastos:igucFXGZDreCg2pEiZTxS4XhM1Bt6rd2Tk");
// the credentials that belong to igucFXGZDreCg2pEiZTxS4XhM1Bt6rd2Tk
VerifiableCredential vcProfile, vcDiploma;

// Create a presentation builder
VerifiablePresentation.Builder pb = VerifiablePresentation.createFor(did);
// create the presentation
VerifiablePresentation vp = pb.id("#demo")
        .credentials(vcProfile)
        .credentials(vcDiploma)
        .realm("https://example.com/")
        .nonce("873172f58701a9ee686f0630204fee59")
        .seal(storePasswd);

// Serialize the presentation
String serializedVp = vp.serialize();
```

对于 VerifiablePresentation 中打包的凭证，只能属于创建 VerifiablePresentation 的 DID 持有人，而不能打包别人的凭证。

The credential packaged in the Verifiable Presentation can only belong to the DID controller who created the Verifiable Presentation. And the credential cannot be packaged by others.

打包的凭证就可以安全的交由验证方，作为个人提交的个人信息凭证。

The packaged credential can be safely handed over to the verifier as the personal information credential submitted by the individual.
