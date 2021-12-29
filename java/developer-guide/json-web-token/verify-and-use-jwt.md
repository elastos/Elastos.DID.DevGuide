# Verify and use JWT

在验证方收到 JWT 以后需要对 JWT 进行验证，从而保证 JWT 的有效性。JWT 的验证可以有两种形式。

After receiving JWT, the verifier needs to verify it, so as to ensure its validity. JWT verification can take two forms.

* 验证 JWT 是一个有效 DID 签名的有效 token （通用验证）
* 验证 JWT 是一个特定 DID 签名的有效 token （特定验证）
* JWT authentication is a valid token of a valid DID signature (universal authentication)
* JWT authentication is a valid token of a specific DID signature (specific authentication)

## 通用验证

Universal authentication

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

在这种情况下，JWT parser 会解析 JWT 中的签名人，并尝试解析其 DID 后基于签名人的 DID 对 JWT 进行验证。

In this case, JWT parser will parse the signer in JWT, and try to verify JWT based on the signer’s DID after parsing its DID.

## 特定验证

Specific authentication

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

这种验证方式，会假定该 token 是有指定的 DID 签名，如果 JWT 的签名 DID 和 signer 的 DID 不一致，会得到验证错误

Here, it is assumed that the token has a specified DID signature. If the DID of JWT and the DID of signer are inconsistent, the result of authentication will be error.

## 读取 JWT 的信息

Read information of JWT

在解析和验证 token 后，Java SDK 会返回一个 JWT 的对象，通过这个对象接口，可以访问 JWT 中封装的属性和数据。例如：

After parsing and verifying the token, Java SDK will return an object of JWT, through which the user can access the attributes and data encapsulated in JWT. Example:

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

详细的 API 信息参见 [javadoc](https://todo/url/to/javadoc)。

See Javadoc for detailed API information.

## Claims assertions (requiring specific values)

DID SDK 提供的 JWT parser 可以支持 claims assertions，即可以指定解析和验证 token 时，对特定的属性有要求，并且需要匹配。示例：

JWT parser provided by DID SDK can support claims assertions, that is, it can be specified that when parsing and verifying token, there are requirements for specific attributes and matching is needed. Example:

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

如果在构建 Parser 的时候声明了 claims assertions，那么该 parser 在解析和验证 JWT token 的时候会对这些 assertions 逐一进行检查，如果和声明不匹配，则会报告 token 解析验证错误。

If claims assertions are declared when the Parser is built, the parser will check these assertions one by one when parsing and verifying JWT token, and if they do not match the declaration, it will report that the token parsing and verification are error.
