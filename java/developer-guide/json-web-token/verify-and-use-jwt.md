# Verify and Use JWT

After receiving JWT, it needs to be reviewed by the verifier to ensure its validity. JWT verification can take two forms:

* JWT authentication is a valid token of a valid DID signature (universal authentication)
* JWT authentication is a valid token of a specific DID signature (specific authentication)

## Universal Authentication

```java
String token; // the JWT token from 3rd party

// Create a JWT parser
JwtParser parser = new JwtParserBuilder().build();
// Parse and verify the token
try {
    Jws<Claims> jwt = parser.parseClaimsJws(token);
} catch (JwtException e) {
    // Handle the parse and verify errors
}
```

In this case, JWT parser will parse the signer in JWT and try to verify it based on the signer’s DID after parsing it.

## Specific Authentication

```java
String token; // the JWT token from 3rd party
DID did = new DID("did:elastos:igyq8SV5RT33HVqgCTCGU48u2pfnyH4XMn"); // expected signer

// resolve signer's DIDDocument
DIDDocument signer = did.resolve();
if （!signer.isValid()) {
    // should report errorss
}

// Create a JWT parser with signer
JwtParser parser = signer.jwtParserBuilder().build();
// Parse and verify the token
try {
    Jws<Claims> jwt = parser.parseClaimsJws(token);
} catch (JwtException e) {
    // Handle the parse and verify errors
}
```

Here, it's assumed that the token has a specified DID signature. If the DID of JWT and the signer are inconsistent, the result of the authentication will be an error.

## Read Information on JWT

After parsing and verifying the token, Java SDK will return an object of JWT, through which the user can access the attributes and data encapsulated in JWT. For example:

```java
Jws<Claims> jwt = parser.parseClaimsJws(token);

// Read the JWT header
JwsHeader header = jwt.getHeader();
String contentType = header.getContentType();
String app = header.get("app")
// ...

// Read the claims
Claims claims = jwt.getBody();
String subject = claims.getSubject();
String audience = claims.getAudience();
// ...
```

See [Javadoc](https://todo/url/to/javadoc) for detailed API information.

## Claims Assertions (Requiring Specific Values)

JWT parser provided by the DID SDK can support claims assertions - that is, it can be specified that when parsing and verifying token. There are requirements for specific attributes, and matching is also needed. For example:

```java
String token; // the JWT token from 3rd party

// Create a JWT parser with claims assertions
JwtParser parser = new JwtParserBuilder()
    .requireId(e3dcc871625955d01d83d9b6edf20b8a)
    .requireSubject("demo")
    // other assertions
    .build();

// Parse and verify the token
try {
    Jws<Claims> jwt = parser.parseClaimsJws(token);
} catch (JwtException e) {
    // Handle the parse and verify errors
}
```

If claims assertions are declared when the Parser is built, the Parser will check these assertions one by one when parsing and verifying JWT token. If they don't match the declaration, it will report that the token parsing and verification have an error.
