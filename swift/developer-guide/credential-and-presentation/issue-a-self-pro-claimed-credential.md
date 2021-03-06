# Issue a Self-proclaimed Credential

A self-pro-claimed credential is a credential issued by the users themselves for self-declaration of specific information. An example of creating self-pro-claimed credentials is as follows:

```
let store: DIDStore =... // an opened DIDStore instance
let storePasswd = "secret"
let did = try DID("did:elastos:iaurnSo71QStq2vXdJCWiqXYZWmR499pjn")
let doc = try store.loadDid(doc)
let selfIssuer = try VerifiableCredentialIssuer(doc)
// Create a credential builder for self-proclaimed credential
let cb = try selfIssuer.editingVerifiableCredentialFor(did: did)
// Create the credential
let vc = cb.withId("#profile")
    .withType("https://elastos.org/credentials/v1#SelfProclaimedCredential")
    .withType("https://elastos.org/credentials/profile/v1#ProfileCredential")
    .withType("EmailCredential", "https://elastos.org/credentials/email/v1")
    .withType("SocialCredential", "https://elastos.org/credentials/social/v1")
    .withProperties("name", "John")
    .withProperties("gender", "Male")
    .withProperties("nationality", "Singapore")
    .withProperties("email", "john@example.com")
    .withProperties("twitter", "@john")
    .seal(using: storePasswd)
// Save the credential to the store for later usage
try store.storeCredential(using: vc)
```

The credential issued by the customized DID is the same as that issued by the ordinary DID.
