# Issue a self-pro-claimed credential

自声明凭证是自己颁发给自己的凭证，用于自我声明特定的信息。创建自声明的凭证示例如下：

A self-pro-claimed credential is a credential issued by oneself for self-declaration of specific information. Example of creating self-pro-claimed credential is as follows:

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID did = new DID("did:elastos:iaurnSo71QStq2vXdJCWiqXYZWmR499pjn");

DIDDocument doc = store.loadDid(doc);
Issuer selfIssuer = new Issuer(doc);

// Create a credential builder for self-proclaimed credential
VerifiableCredential.Builder cb = selfIssuer.issueFor(did);
// Create the credential
VerifiableCredential vc = cb.id("#profile")
    .type("https://elastos.org/credentials/v1#SelfProclaimedCredential")
    .type("https://elastos.org/credentials/profile/v1#ProfileCredential")
    .type("EmailCredential", "https://elastos.org/credentials/email/v1")
    .type("SocialCredential", "https://elastos.org/credentials/social/v1")
    .property("name", "John");
    .property("gender", "Male");
    .property("nationality", "Singapore");
    .property("email", "john@example.com");
    .property("twitter", "@john");
    .seal(storePasswd);

// Save the credential to the store for later usage
store.storeCredential(vc);
```

自定义 DID 颁发凭证和普通 DID 一致。

The credential issued by customized DID is consistent with primitive DID.
