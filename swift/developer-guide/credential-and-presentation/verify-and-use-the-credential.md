# How to Verify and Use the credential

VerifiableCredential is a sealed object. Once created, the credential is protected by the issuer’s signature and cannot be modified - any modification will destroy its integrity. Before using the credential, its validity should be verified, and only when it is valid can the information contained in the credential be admitted and used.

## Verify the Credential

The credential can be verified from the following three perspectives.

### Expiration

The credential has a validity period when it's created and is recorded in the corresponding VerifiableCredential object. The credential represents a valid identity only when it's within its validity period. The following methods can be used to verify whether a credential is within its validity period:

```
let serializedVc: String = ... // a serialized credential
let vc = try VerifiableCredential.fromJson(serializedVc)
let expired = try vc.isExpired()
```

### Revoked

Credentials can be revoked where they cannot be used or accepted by third parties. The following methods can be used to test whether a credential is revoked:

```
let serializedVc: String = ... // a serialized credential
let vc = try VerifiableCredential.fromJson(serializedVc)
let revoked = try vc.isRevoked()
```

See [Issue a credential](publish-credential.md) to get to know how to revoke a credential.

### Genuine

After obtaining a credential, it's also necessary to test whether this document has been maliciously modified. The test method is as follows:

```
let serializedVc: String = ... // a serialized credential
let vc = try VerifiableCredential.fromJson(serializedVc)
let genuine = try vc.isGenuine()
```

In daily use, the above three aspects of a credential should be tested to accurately verify its validity. The DID SDK offers a composite method isValid (), which contains all the verifications. For example:

```
let serializedVc: String = ... // a serialized credential
let vc = try VerifiableCredential.fromJson(serializedVc)
let valid = try vc.isValid()
```

## Read the Credential Information

VerifiableCredential provides a series of methods to read the credential information. For example:

```
let serializedVc: String = ... // a serialized credential
let vc = try VerifiableCredential.fromJson(serializedVc)
// get the issuer
let issuer = vc.issuer
// get the subject
let subject = vc.subject
// get the subject content
let name = subject.getPropertyAsString("name")
// or
let props = subject.properties()
```
