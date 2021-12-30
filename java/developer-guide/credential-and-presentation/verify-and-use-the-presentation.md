# How to Verify and Use the Presentation

Verifiable Presentation is a sealed object that is protected by the creator’s signature after creation, and can’t be modified at all - all modifications will destroy its integrity. Before using the Verifiable Presentation, it's important to verify its validity and that of the credential contained therein so that it can be accepted and used only if it's valid.

## Verify Presentation

The verification of the presentation needs to include the signature of the presentation itself, as well as the verification of all internal credentials. The isValid () method of Verifiable Presentation includes the verification of all the above items. For example:

```java
String serializedVp; // a serialized presentation
VerifiablePresentation vp = VerifiablePresentation.parse(serializedVp);
boolean valid = vp.isValid();
```

## Information Reading of Verifiable Presentation

Verifiable Presentation provides a method to read the contained credentials and verify the information. As an example:

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
