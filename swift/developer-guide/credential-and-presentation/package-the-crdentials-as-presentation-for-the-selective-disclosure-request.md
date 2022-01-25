# Package Credentials as Presentation for the Selective Disclosure Request

For security reasons, the credentials cannot be handed over directly by the holder to the verifier as the personal information certificate. The credentials to be submitted to the verifier should be packaged as VerifiablePresentation, which will include the purpose of the credentials, the authentication session, and the signature of the holder of the valid credentials.

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

The credentials packaged in the VerifiablePresentation only belong to the DID holder who created it, while the credentials of others cannot be packaged. The packaged credentials can be safely handed over to the verifier as the personal information credentials submitted by individuals.
