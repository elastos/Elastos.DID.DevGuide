# How to Verify and Use the Credential

Verifiable Credential is a sealed object. After a credential is created, it is protected by the issuer’s signature - it cannot be modified (all modifications will destroy its integrity). Before using the credential, the validity needs to be verified. The information contained in the credential can only be used on the premise of its validity.

## Credential Authentication

The credential can be verified from the following three aspects:

### Expiration

A credential has an effective period when it is created, which is recorded in the corresponding Verifiable Credential object. Only when the credential is within the effective period can it be a valid identity. For any credential, you can verify whether it is in the effective period by the following methods:

```java
String serializedVc; // a serialized credential
VerifiableCredential vc = VerifiableCredential.parse(serializedVc);
boolean expired = vc.isExpired();
```

### Revoked

The credential can be revoked, meaning it cannot be used and accepted by third parties. You can test whether the credential has been revoked by using the following methods: &#x20;

```java
String serializedVc; // a serialized credential
VerifiableCredential vc = VerifiableCredential.parse(serializedVc);
boolean revoked = vc.isRevoked();
```

See [publishing credential](publish-credential.md) for how to revoke credential.

### Genuine

After obtaining a credential object, it's also necessary to test whether the document has been maliciously modified. The test method is as follows:

```java
String serializedVc; // a serialized credential
VerifiableCredential vc = VerifiableCredential.parse(serializedVc);
boolean genuine = vc.isGenuine();
```

In daily use, to verify the validity of a credential, it's necessary to verify the above three aspects in order to know the accurate result. The DID SDK provides a composite method isValid (), which contains all the verifications. For example:

```java
String serializedVc; // a serialized credential
VerifiableCredential vc = VerifiableCredential.parse(serializedVc);
boolean valid = vc.isValid();
```

## Credential Information Reading

Verifiable Credential provides a series of methods to read the information of credentials. For example:

```java
String serializedVc; // a serialized credential
VerifiableCredential vc = VerifiableCredential.parse(serializedVc);

// get the issuer
DID issuer = vc.getIssuer();
// get the subject
VerifiableCredential.Subject subject = vc.getSubject();
// get the subject content
String name = subject.getProperty("name");
// or
Map<String, Object> props = subject.getProperties();
```
