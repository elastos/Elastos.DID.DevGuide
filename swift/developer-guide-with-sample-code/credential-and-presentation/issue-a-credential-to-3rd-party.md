# Issue a credential to 3rd party

给第三方颁发凭证的场景主要用于应用服务给用户颁发凭证，或者 KYC 机构给第三个实体颁发经过认证的个人信息的凭证。示例如下：

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

为了保证凭证可验证，颁发凭证的实体的 DID 需要发布上链。

自定义 DID 颁发凭证，或者给自定义 DID 颁发凭证和普通 DID 一致。
