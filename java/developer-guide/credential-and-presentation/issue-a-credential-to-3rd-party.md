# Issue a Credential to a 3rd Party

Issuing credentials to third parties is mainly used for application services to issue credentials to users, or KYC institutions to issue credentials of certified personal information to third entities. An example is as follows:

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID example = new DID("did:elastos:examplekyc");

DIDDocument doc = store.loadDid(example);
Issuer kycIssuer = new Issuer(doc);

// Create a credential builder for 3rd party: 
//        did:elastos:igHQ6Mfp3RouLtqeiv7D3Uz5JDyVrW4W3A
VerifiableCredential.Builder cb = selfIssuer.issueFor(
        "did:elastos:igHQ6Mfp3RouLtqeiv7D3Uz5JDyVrW4W3A");
// Create the credential
VerifiableCredential vc = cb.id("#profile")
    .type("https://example.com/credentials/v1#KycCredential")
    .type("https://elastos.org/credentials/profile/v1#ProfileCredential")
    .type("EmailCredential", "https://elastos.org/credentials/email/v1")
    .type("SocialCredential", "https://elastos.org/credentials/social/v1")
    .property("name", "John");
    .property("gender", "Male");
    .property("nationality", "Singapore");
    .property("email", "john@example.com");
    .property("twitter", "@john");
    .seal(storePasswd);

// or serialize the credential to string, send to the 3rd party in secure way
String serializedVc = vc.serialize();
```

To ensure that the credential can be verified, the DID of the entity issuing the credential needs to be published and uploaded to the chain.

A credential issued by customized DID, or one issued to customized DID, is consistent with primitive DID.
