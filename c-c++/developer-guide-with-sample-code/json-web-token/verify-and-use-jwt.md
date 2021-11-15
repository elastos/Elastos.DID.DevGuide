# Verify and use jwt

SDK提供方法分析JWT token类型（JWT/JWS），若是JWS会做相应的验签后解析，最后JWT和JWS token string都转换JWT。

JWT提供多种方法获取JWT 元素供用户使用和参考。

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

该方法由DID Document提供，获取JWSParser object。`document`提供key用于验签token。

```c
void JWSParser_Destroy(JWSParser *parser);
```

用完JWSParser object，使用该方法销毁。

```c
JWT *JWTParser_Parse(const char *token);
```

该方法解析JWT Token，即不含签名的Token。若成功，返回JWT object，否则报错。

```c
JWT *DefaultJWSParser_Parse(const char *token);
```

该方法根据token只自带的Key的所有者生成解析器去解析`token`，获得JWT object。

```c
JWT *JWSParser_Parse(JWSParser *parser, const char *token);
```

该方法根据用户提供的`parser`来解析token，获得JWT object。

```c
void JWT_Destroy(JWT *jwt);
```

用完JWT object，使用该方法销毁。

```c
const char *JWT_GetHeader(JWT *jwt, const char *attr);
```

该方法可获取JWT中header中各元素内容。SDK还提供直接获取基本属性方法，比如`JWT_GetAlgorithm`,`JWT_GetKeyId`等，具体详见API文档。

```c
const char *JWT_GetClaim(JWT *jwt, const char *key);
```

该方法可获取JWT中claims中各元素内容，内容以字符串返回。有些内容非字符串，SDK提供相应不同返回类型的API，`JWT_GetClaimAsJson`,`JWT_GetClaimAsInteger`,`JWT_GetClaimAsBoolean`。

对于claim中一些基本属性，SDK也提供相应方法，比如`JWT_GetIssuer`,`JWT_GetSubject`等，具体详见API文档。
