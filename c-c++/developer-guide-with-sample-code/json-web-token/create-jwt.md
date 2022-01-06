# Create JWT

JWT内容通过JWTBuilder来编译，JWTBuilder分为JWTHeader和Claims两个部分，分别填充，最后根据是否签名封装，得到JWT或者JWS Token String。

JWT content is compiled by JWTBuilder, which is divided into two parts: JWTHeader and Claims. These two parts are filled separately. JWT or JWS Token String is obtained eventually according to whether the package is signed or not.

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

For better understanding, this method is introduced in JWT module. DID Document provides a method to create JWTBuilder and get the JWT compilation mode.

```c
void JWTBuilder_Destroy(JWTBuilder *builder);
```

用完JWTBuilder object，需要销毁。

JWTBuilder object needs to be destroyed after using.

```c
bool JWTBuilder_SetHeader(
        JWTBuilder *builder,
        const char *attr, const char *value);
```

该方法添加JWT中header部分的元素，但是不可添加默认属性"alg"和"kid"，否则失败。

This method adds elements in the header of JWT, but the default attributes “alg” and “kid” cannot be added, otherwise it will fail.

```c
bool JWTBuilder_SetClaim(
        JWTBuilder *builder,
        const char *key, const char *value);
```

该方法添加JWT中Claims部分的元素，若添加元素的value类型非字符串，还可以选择`JWTBuilder_SetClaimWithJson`,`JWTBuilder_SetClaimWithIntegar`和`JWTBuilder_SetClaimWithBoolean`，具体使用方法参考API文档。

This method adds the elements of Claims in JWT. If the value type of the added elements is not string, you can also choose JWT Builder \_ SetClaimwithJSON, JWT Builder \_ SetClaimwithIntegar and JWTBuilder\_SetClaimWithBoolean. Refer to the API documentation for specific methods.

```c
bool JWTBuilder_SetSubject(JWTBuilder *builder, const char *subject);
```

该方法设置"sub"属性，SDK提供一些固定属性设置，具体可见API文档。

This method sets the “sub” attribute, and SDK provides some fixed attribute settings, which can be found in the API document.

```c
int JWTBuilder_Sign(
        JWTBuilder *builder,
        DIDURL *keyid,
        const char *storepass);
```

该方法对JWTBuilder内容签名，生成JWS字符串。

This method signs JWTBuilder content and generates JWS string.

keyid为创建JWTBuilder的DID Document里的Authentication Key。

_Keyid_ is the Authentication Key in DID Document that created JWTBuilder.

keyid为创建JWTBuilder的DID Document

_Keyid_ is DID Document that created JWTBuilder.

```c
const char *JWTBuilder_Compact(JWTBuilder *builder);
```

该方法对JWTBuilder内容封装，生成JWT字符串。注意：用完返回值字符串，释放。

This method encapsulates JWTBuilder content and generates JWT string. Attetion: the return value string needs to be released after using.

```c
int JWTBuilder_Reset(JWTBuilder *builder);
```

重设header and body部分，除了“alg”，“kid” 和“iss”。

Reset the header and body, except “alg”, “kid” and “iss”.
