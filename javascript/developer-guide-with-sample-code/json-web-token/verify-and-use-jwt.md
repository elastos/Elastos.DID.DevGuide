# Verify and Use JWT

The SDK provides a method to analyze the type of JWToken (JWT/JWS). If JWS does the corresponding post-verification parsing, both JWT and JWS token string will convert JWT.

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

This method is provided by the DID document to obtain the JWTParserBuilder object. JWTParserBuilder enables users to set the filter options to check whether the token meets the requirements or not. The interfaces include requireSubject, requireAudience, requireIssuer, requireIssuedAt, requireAlgorithms, requireHeaderType, and setAllowedClockSkewSeconds. Refer to the API document for specifics.

```typescript
public build(): JWTParser;
```

This method is provided by JWTParserBuilder to encapsulate JWTParser.

```typescript
public async parse(token: string): Promise<JWT>；
```

This method is provided by JWTParser to parse the token. If succeeds, return JWT object; otherwise, an error is returned.

```typescript
public getHeader(): JWTHeader；
```

This method is offered by JWT to get the content of JWT header.

```typescript
public getBody(): Claims；
```

This method is provided by JWT to get the content of JWT header body.
