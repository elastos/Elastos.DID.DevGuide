# Verify and use JWT

在验证方收到 JWT 以后需要对 JWT 进行验证，从而保证 JWT 的有效性。JWT 的验证可以有两种形式。

- 验证 JWT 是一个有效 DID 签名的有效 token （通用验证）
- 验证 JWT 是一个特定 DID 签名的有效 token （特定验证）

## 通用验证

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

在这种情况下，JWT parser 会解析 JWT 中的签名人，并尝试解析其 DID 后基于签名人的 DID 对 JWT 进行验证。

## 特定验证

```
let token: DIDStore = ...  // the JWT token from 3rd party
let did = try DID("did:elastos:igyq8SV5RT33HVqgCTCGU48u2pfnyH4XMn") // expected signer

// resolve signer's DIDDocument
let signer = try did.resolve()
if try !signer.isValid() {
    // should report errorss  
}

// Create a JWT parser with signer
let parser = signer.jwtParserBuilder().build()
// Parse and verify the token
do {
	  let jwt = try parser.parseClaimsJwt(token)
} catch {
    // Handle the parse and verify errors
}
```

这种验证方式，会假定该 token 是有指定的 DID 签名，如果 JWT 的签名 DID 和 signer 的 DID 不一致，会得到验证错误

## 读取 JWT 的信息

在解析和验证 token 后，Java SDK 会返回一个 JWT 的对象，通过这个对象接口，可以访问 JWT 中封装的属性和数据。例如：

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


