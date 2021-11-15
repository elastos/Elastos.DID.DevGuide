# Issue a credential to 3rd party

给第三方颁发凭证的场景主要用于应用服务给用户颁发凭证，或者 KYC 机构给第三个实体颁发经过认证的个人信息的凭证。示例如下：

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

为了保证凭证可验证，颁发凭证的实体的 DID 需要发布上链。

自定义 DID 颁发凭证，或者给自定义 DID 颁发凭证和普通 DID 一致。
