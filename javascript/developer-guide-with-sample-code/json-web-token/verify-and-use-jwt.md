# Verify and use jwt

SDK提供方法分析JWT token类型（JWT/JWS），若是JWS会做相应的验签后解析，最后JWT和JWS token string都转换JWT。

SDK provides a method to analyze the type of JWToken (JWT/JWS). If JWS does the corresponding post-verification parsing, both JWT and JWS token string will convert JWT.

JWT提供多种方法获取JWT 元素供用户使用和参考。

JWT provides a multitude of methods to obtain JWT elements for the usage and reference of users.

## Example

```typescript
let jpb = doc.jwtParserBuilder();
let jp = jpb.requireSubject(doc.getSubject()).build();

let jwt =  await jp.parse(token);
expect(jwt).not.toBeNull();
h = jwt.getHeader();
//get each elem of head
console.log("get header type: {}", h.getType());
... ... ... ...
//get each elem of body
let c = jwt.getBody();
console.log("get issuer: {}", c.getIssuer());
... ... ... ...
```

## Usage

```typescript
public jwtParserBuilder(): JWTParserBuilder;
```

该方法由DID Document提供，获取JWTParserBuilder object。 JWTParserBuilder可供用户设置过滤选项，用于检查token是否符合要求，接口有requireSubject，requireAudience，requireIssuer，requireIssuedAt，requireAlgorithms，requireHeaderType和setAllowedClockSkewSeconds，具体使用详见API文档。

This method is provided by the DID document to obtain the JWTParserBuilder object. JWTParserBuilder enables users to set the filter options to check whether the token meets the requirements or not. The interfaces include requireSubject, requireAudience, requireIssuer, requireIssuedAt, requireAlgorithms, requireHeaderType and setAllowedClockSkewSeconds. Refer to the API document for specifics.

```typescript
public build(): JWTParser;
```

该方法由JWTParserBuilder提供，用于封装JWTParser。

This method is provided by JWTParserBuilder to encapsulate JWTParser.

```typescript
public async parse(token: string): Promise<JWT>；
```

该方法由JWTParser提供，解析token。若成功，返回JWT object，否则报错。

This method is provided by JWTParser to parse the token. If succeeds, return JWT object; otherwise, an error is returned.

```typescript
public getHeader(): JWTHeader；
```

该方法由JWT提供，用于获取JWT header的内容。

This method is offered by JWT to get the content of JWT header.

```typescript
public getBody(): Claims；
```

该方法由JWT提供，用于获取JWT header body（主体）内容。

This method is provided by JWT to get the content of JWT header body.
