# Create JWT

Elastos DID 可以从 DID 文档中创建一个 JWT 的 Builder 对象，通过这个 Builder 对象，透明的实现使用 DID 的密钥对 JWT 的签名。

创建 JWT 的示例：

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

上述示例生成的 JWT 就是由 DID `did:elastos:imxZBD88s7i7sR2RdKd6YTHB72kCyMAsFd` 进行签名并序列化的 token。
