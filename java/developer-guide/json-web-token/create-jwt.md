# Create JWT

Elastos DID can create a Builder object of JWT from the DID document. Through this Builder object, it can transparently sign JWT with the DID’s key.

Example of creating JWT:

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID did = new DID("did:elastos:imxZBD88s7i7sR2RdKd6YTHB72kCyMAsFd");

Calendar cal = Calendar.getInstance();
cal.set(Calendar.MILLISECOND, 0);
Date iat = cal.getTime();
cal.add(Calendar.MONTH, -1);
Date nbf = cal.getTime();
cal.add(Calendar.MONTH, 1);
Date exp = cal.getTime();

// Get the DIDDocument
DIDDocument doc = store.loadDid(did);
// Create JwtBuilder with the DID
JwtBuilder builder = doc.jwtBuilder();

// Create a signed token
String token = builder.addHeader(Header.TYPE, Header.JWT_TYPE)
        .addHeader(Header.CONTENT_TYPE, "application/json")
        .addHeader("app", "DID Samples")
        .setSubject("demo")
        .setId("e3dcc871625955d01d83d9b6edf20b8a")
        .setAudience("did:elastos:igyq8SV5RT33HVqgCTCGU48u2pfnyH4XMn")
        .setIssuedAt(iat)
        .setExpiration(exp)
        .setNotBefore(nbf)
        .claim("foo", "bar")
        .sign(storePasswd)
        .compact();
```

The JWT generated from the above example is a token signed and serialized by DID did:elastos:imxzbd88s7i7sr2rdkd6ythb72kymasfd.
