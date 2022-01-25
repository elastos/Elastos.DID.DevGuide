# Verify and Use JWT

The SDK provides a method to analyze the type of JWT token (JWT/JWS). If JWS will do the corresponding post-verification parsing, both the JWT and JWS token strings will finally convert to JWT.

JWT provides a variety of methods to obtain JWT elements for user reference.

## Example

```c
//token is jwt token
const char *token = "eyJ0eXAiOiJKV1QiLCJjdHkiOiJqc29uIiwibGlicmFyeSI6IkVsYXN0b3MgRElEIiwidmVyc2lvbiI6IjEuMCIsImFsZyI6Im5vbmUifQ.eyJzdWIiOiJKd3RUZXN0IiwianRpIjoiMCIsImF1ZCI6IlRlc3QgY2FzZXMiLCJpYXQiOjE1OTA1NjE1MDQsImV4cCI6MTU5ODUxMDMwNCwibmJmIjoxNTg3OTY5NTA0LCJmb28iOiJiYXIiLCJpc3MiOiJkaWQ6ZWxhc3RvczppV0ZBVVloVGEzNWMxZlBlM2lDSnZpaFpIeDZxdXVtbnltIn0.";

JWT *jwt = JWTParser_Parse(token);
... ... ... ...
JWT_Destroy(jwt);
  
//token is jws token
token = "eyJ0eXAiOiJKV1QiLCJjdHkiOiJqc29uIiwibGlicmFyeSI6IkVsYXN0b3MgRElEIiwidmVyc2lvbiI6IjEuMCIsImFsZyI6IkVTMjU2In0.eyJzdWIiOiJKd3RUZXN0IiwianRpIjoiMCIsImF1ZCI6IlRlc3QgY2FzZXMiLCJpYXQiOjE2MDAwNzM4MzQsImV4cCI6MTc1NTE2MTgzNCwibmJmIjoxNTk3Mzk1NDM0LCJmb28iOiJiYXIiLCJpc3MiOiJkaWQ6ZWxhc3RvczppV0ZBVVloVGEzNWMxZlBlM2lDSnZpaFpIeDZxdXVtbnltIn0.rW6lGLpsGQJ7kojql78rX7p-MnBMBGEcBXYHkw_heisv7eEic574qL-0Immh0f0qFygNHY7RwhL47PDtFyNHAA";
jwt = DefaultJWSParser_Parse(token);
... ... ... ...
JWT_Destroy(jwt);
```

## Usage

```c
JWSParser *DIDDocument_GetJwsParser(DIDDocument *document);
```

The method is provided by DID Document and gets JWSParser object. document provides key to verify the token.

```c
void JWSParser_Destroy(JWSParser *parser);
```

Use this method to destroy the JWSParser object after using it.

```c
JWT *JWTParser_Parse(const char *token);
```

This method parses JWT Token, or a token without a signature. If it's successful, the JWT object will be returned; otherwise, an error will be reported.

```c
JWT *DefaultJWSParser_Parse(const char *token);
```

This method generates a parser to parse the token according to the owner of the Key that comes with the token and obtains JWT object.

```c
JWT *JWSParser_Parse(JWSParser *parser, const char *token);
```

This method parses the token according to the parser provided by the user to obtain JWT object.

```c
void JWT_Destroy(JWT *jwt);
```

Use this method to destroy JWT object after using it.

```c
const char *JWT_GetHeader(JWT *jwt, const char *attr);
```

This method can get the contents of each element in the header in JWT. SDK also provide methods to directly obtain basic attribute, such as JWT\_GetAlgorithm,JWT\_GetKeyId. See API documentation for details.

```c
const char *JWT_GetClaim(JWT *jwt, const char *key);
```

This method can get the content of each element in claims in JWT, and the content is returned as a string. Some contents are not strings, and SDK provides APIs of different return types, JWT\_ GetClaimAsJson, JWT\_ GetClaimAsInteger, JWT\_ GetClaimAsBoolean.

For some basic attributes in claim, SDK also provides corresponding methods, such as JWT\_GetIssuer and JWT\_GetSubject. See API documentation for details.
