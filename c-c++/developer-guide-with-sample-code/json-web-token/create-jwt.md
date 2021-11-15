# Create JWT

JWT内容通过JWTBuilder来编译，JWTBuilder分为JWTHeader和Claims两个部分，分别填充，最后根据是否签名封装，得到JWT或者JWS Token String。

## Example

```c
JWTBuilder *builder = DIDDocument_GetJwtBuilder(doc);
if（!builder）
  	//error operation

if (!JWTBuilder_SetHeader(builder, "ctyp", "json") || 
    		!JWTBuilder_SetHeader(builder, "library", "Elastos DID") ||
				!JWTBuilder_SetHeader(builder, "typ", "JWT") ||
				!JWTBuilder_SetHeader(builder, "version", "1.0"))
    //error operation

const char *json = "{\"hello\":\"world\",\"test\":\"true\"}";

if (!JWTBuilder_SetSubject(builder, "JwtTest") || !JWTBuilder_SetId(builder, "0") ||
				!JWTBuilder_SetAudience(builder, "Test cases") || !JWTBuilder_SetIssuedAt(builder, iat) ||
				!JWTBuilder_SetExpiration(builder, exp) || !JWTBuilder_SetNotBefore(builder, nbf) ||
				!JWTBuilder_SetClaim(builder, "foo", "bar") || !JWTBuilder_SetClaimWithJson(builder, "object", json) ||
				!JWTBuilder_SetClaimWithBoolean(builder, "finished", false))
     //error operation

const char *token = JWTBuilder_Compact(builder);
JWTBuilder_Destroy(builder);
... ... ... ...
free((void*)token);
```

## Usage

```c
JWTBuilder *DIDDocument_GetJwtBuilder(DIDDocument *document);
```

为了更好的理解，把该方法放到JWT模块中介绍。DID Document提供了方法创建JWTBuilder，得到JWT编译模式。

```c
void JWTBuilder_Destroy(JWTBuilder *builder);
```

用完JWTBuilder object，需要销毁。

```c
bool JWTBuilder_SetHeader(JWTBuilder *builder, const char *attr, const char *value);
```

该方法添加JWT中header部分的元素，但是不可添加默认属性"alg"和"kid"，否则失败。

```c
bool JWTBuilder_SetClaim(JWTBuilder *builder, const char *key, const char *value);
```

该方法添加JWT中Claims部分的元素，若添加元素的value类型非字符串，还可以选择`JWTBuilder_SetClaimWithJson`,`JWTBuilder_SetClaimWithIntegar`和`JWTBuilder_SetClaimWithBoolean`，具体使用方法参考API文档。

```c
bool JWTBuilder_SetSubject(JWTBuilder *builder, const char *subject);
```

该方法设置"sub"属性，SDK提供一些固定属性设置，具体可见API文档。

```c
int JWTBuilder_Sign(JWTBuilder *builder, DIDURL *keyid, const char *storepass);
```

该方法对JWTBuilder内容签名，生成JWS字符串。

keyid为创建JWTBuilder的DID Document里的Authentication Key。

keyid为创建JWTBuilder的DID Document

```c
const char *JWTBuilder_Compact(JWTBuilder *builder);
```

该方法对JWTBuilder内容封装，生成JWT字符串。注意：用完返回值字符串，释放。

```c
int JWTBuilder_Reset(JWTBuilder *builder);
```

重设header and body部分，除了“alg”，“kid” 和“iss”。
