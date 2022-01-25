# Package the Credentials as Presentation for the Selective Disclosure Request

For security reasons, the credential cannot be directly handed over as personal information to the verifier by the controller. The credential to be submitted to the verifier needs to be packaged as a Verifiable Presentation, which will include the purpose of the credential, identification of the verification session, the signature of the valid credential controller, and other information.

An example of packaging credential as Verifiable Presentation is as follows:

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

The credential packaged in the Verifiable Presentation can only belong to the DID controller who created it - the credential cannot be packaged by others. The packaged credential can be safely handed over to the verifier as the personal information credential submitted by the individual.
