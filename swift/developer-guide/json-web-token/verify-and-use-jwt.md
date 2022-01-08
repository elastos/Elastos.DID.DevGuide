# Verify and Use JWT

Upon receiving the JWT, the verifier needs to validate it. The JWT can be verified in the following two ways.

* Validate whether the JWT is a valid token of a valid DID signature (generic validation)
* Validate whether the JWT is a valid token of a specified DID signature (specified validation)

## Generic Validation

```
let token: String = ... // the JWT token from 3rd party
// Create a JWT parser
let parser = try JwtParserBuilder().build()
// Parse and verify the token
do {
	  let jwt = try parser.parseClaimsJwt(token)
} catch {
    // Handle the parse and verify errors
}
```

Under such circumstances, the JWT parser will resolve the signer in the JWT and try verifying the JWT based on the signer’s DID after resolving its DID.

## Specified Validation

```
let token: DIDStore = ...  // the JWT token from 3rd party
let did = try DID("did:elastos:igyq8SV5RT33HVqgCTCGU48u2pfnyH4XMn") // expected signer
// resolve signer's DIDDocument
let signer = try did.resolve()
if try !signer.isValid() {
    // should report errorss  
// Create a JWT parser with signer
let parser = signer.jwtParserBuilder().build()
// Parse and verify the token
do {
	  let jwt = try parser.parseClaimsJwt(token)
} catch {
    // Handle the parse and verify errors
```

In this case, this token is assumed to have a specified DID signature. If the signed DID of JWT is inconsistent with the signer’s DID, the verification will fail.

## Read JWT Information

After resolving and validating the token, Swift SDK will return a JWT object through the interface of which the attributes and data encapsulated in the JWT can be accessed. For example:

```
let jwt = parser.parseClaimsJwt(token)
// Read the JWT header
let header = jwt.header
let contentType = header.getContentType()
let version = header.get("version")
// ...
// Read the claims
let claims = jwt.claims
let subject = claims.getSubject()
let audience = claims.getAudience()
// ...
```
