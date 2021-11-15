# Create JWT

Elastos DID 可以从 DID 文档中创建一个 JWT 的 Builder 对象，通过这个 Builder 对象，透明的实现使用 DID 的密钥对 JWT 的签名。

创建 JWT 的示例：

```
let store: DIDStore // an opened DIDStore instance
let storePasswd = "secret"
let did = try DID("did:elastos:imxZBD88s7i7sR2RdKd6YTHB72kCyMAsFd")
let userCalendar = Calendar.current
let components = DateComponents()
components.year = 2020
components.month = 9
components.day = 14
components.minute = 21
components.hour = 21
components.second = 41
let iat = userCalendar.date(from: components)

// Get the DIDDocument
let doc = try store.loadDid(did)
// Create JwtBuilder with the DID
JwtBuilder builder = try doc.jwtBuilder()

// Create a signed token
let token = builder.addHeader(key: Header.TYPE, value: Header.JWT_TYPE)
				.addHeader(key: Header.CONTENT_TYPE, value: "json")
				.addHeader(key: "version", value: "1.0")
				.setSubject(sub: "demo")
				.setId(id: "e3dcc871625955d01d83d9b6edf20b8a")
				.setAudience(audience: "did:elastos:igyq8SV5RT33HVqgCTCGU48u2pfnyH4XMn")
				.setIssuedAt(issuedAt: iat)
				.setExpiration(expiration: exp)
				.setNotBefore(nbf: nbf)
				.claim(name: "foo", value: "bar")
				.sign(using: storePasswd)
				.compact()
```

上述示例生成的 JWT 就是由 DID `did:elastos:imxZBD88s7i7sR2RdKd6YTHB72kCyMAsFd` 进行签名并序列化的 token。
