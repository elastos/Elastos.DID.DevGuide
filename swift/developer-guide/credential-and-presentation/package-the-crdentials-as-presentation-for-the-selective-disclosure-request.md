# Package the crdentials as presentation for the selective disclosure request

出于安全的考虑，凭证不能直接交由持有人交给验证方，作为个人信息凭证。需要提交给验证方的凭证需要打包为 VerifiablePresentation，在 VerifiablePresentation 中会包含凭证的用途，验证会话标识等信息，以及有效凭证持有人的签名。

For security reasons, the credentials cannot be handed over directly by the holder to the verifier as the personal information certificate. The credentials to be submitted to the verifier should be packaged as VerifiablePresentation, which will include the purpose of the credentials, the authentication session, and the signature of the holder of the valid credentials.

将凭证打包为 VerifiablePresentation 的示例如下：

Below is an example of packaging the credentials as VerifiablePresentation:

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let did = try DID("did:elastos:igucFXGZDreCg2pEiZTxS4XhM1Bt6rd2Tk")
// the credentials that belong to igucFXGZDreCg2pEiZTxS4XhM1Bt6rd2Tk

let vcProfile: VerifiableCredential = ...
let vcDiploma: VerifiableCredential = ...

// Create a presentation builder
let pb = VerifiablePresentation.editingVerifiablePresentation(for: did, using: store)
// create the presentation
let vp = pb.withId("#demo")
        .withCredentials(vcProfile, vcDiploma)
        .withRealm("https://example.com/")
        .withNonce("873172f58701a9ee686f0630204fee59")
        .seal(using: storePasswd)

// Serialize the presentation
String serializedVp = vp.serialize();
```

对于 VerifiablePresentation 中打包的凭证，只能属于创建 VerifiablePresentation 的 DID 持有人，而不能打包别人的凭证。

The credentials packaged in the VerifiablePresentation only belong to the DID holder who created the VerifiablePresentation, while the credentials of others cannot be packaged.

打包的凭证就可以安全的交由验证方，作为个人提交的个人信息凭证。

The packaged credentials can be safely handed over to the verifier as the personal information credentials submitted by individuals.
