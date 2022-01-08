# How to Verify and Use the Presentation

VerifiablePresentation is a sealed object. Once created, the credential is protected by the creator’s signature and cannot be modified, because any modification will destroy its integrity. Before using the VerifiablePresentation, it's necessary to verify whether the presentation and the credentials it contains are valid, and only when the validity is confirmed can the information contained in the credentials be admitted and used.

## Verify the Presentation

To verify the presentation, we need to include the signature of the presentation itself and validate all the credentials contained in the presentation. The isValid () method of VerifiablePresentation includes the verification of all the above items. For example:

```
let serializedVp: String = ... // a serialized presentation
VerifiablePresentation vp = try VerifiablePresentation.fromJson(serializedVp);
let valid = try vp.isValid()
```

## Read the Information on VerifiablePresentation

VerifiablePresentation offers an approach to read the credentials and relevant verification information. As an example:

```
let serializedVp: String = ... // a serialized presentation
let vp = try VerifiablePresentation.fromJson(serializedVp)
// get the holder
let holder = vp.holder
// get the embedded credentials
let vcs = vp.credentials
// get the Proof
let proof = vp.proof
let realm = proof.realm
let nonce = proof.nonce
```
