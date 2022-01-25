# Issue a Credential to a Third Party

The scenario of issuing a credential to a third party is mainly used for applications to issue credentials to users, or for KYC institutions to issue credentials of certified personal information to third entities. The following contain some examples:

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let example = try DID("did:elastos:examplekyc")
let doc = try store.loadDid(example)
let kycIssuer = try VerifiableCredentialIssuer(doc)
// Create a credential builder for 3rd party: 
//        did:elastos:igHQ6Mfp3RouLtqeiv7D3Uz5JDyVrW4W3A
let cb = kycIssuer.editingVerifiableCredentialFor(did:
        "did:elastos:igHQ6Mfp3RouLtqeiv7D3Uz5JDyVrW4W3A")
// Create the credential
let vc = cb.withId("#profile")
           .withType("https://example.com/credentials/v1#KycCredential")
           .withType("https://elastos.org/credentials/profile/v1#ProfileCredential")
           .withType("EmailCredential", "https://elastos.org/credentials/email/v1")
           .withType("SocialCredential", "https://elastos.org/credentials/social/v1")
           .withProperties("name", "John")
           .withProperties("gender", "Male")
           .withProperties("nationality", "Singapore")
           .withProperties("email", "john@example.com")
           .withProperties("twitter", "@john")
   			 .seal(using: storePasswd)
// or serialize the credential to string, send to the 3rd party in secure way
let serializedVc = vc.description
```

To ensure that the credential is verifiable, the DID of the entity issuing the certificate needs to be published and uploaded.

The credential issued by the customized DID is the same as that issued by the ordinary DID, and the process of issuing the credential for the customized DID is the same as that for the ordinary DID.
