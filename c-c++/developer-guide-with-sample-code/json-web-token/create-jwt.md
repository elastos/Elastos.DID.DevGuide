# Create JWT

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

To gain a better understanding, this method is introduced in JWT module. DID Document provides a method to create JWTBuilder and get the JWT compilation mode.

```c
void JWTBuilder_Destroy(JWTBuilder *builder);
```

JWTBuilder object needs to be destroyed after using.

```c
bool JWTBuilder_SetHeader(
        JWTBuilder *builder,
        const char *attr, const char *value);
```

This method adds elements in the header of JWT, but the default attributes “alg” and “kid” cannot be added, otherwise it will fail.

```c
bool JWTBuilder_SetClaim(
        JWTBuilder *builder,
        const char *key, const char *value);
```

This method adds the elements of Claims in JWT. If the value type of the added elements is not string, you can also choose JWT Builder \_ SetClaimwithJSON, JWT Builder \_ SetClaimwithIntegar and JWTBuilder\_SetClaimWithBoolean. Refer to the API documentation for specific methods.

```c
bool JWTBuilder_SetSubject(JWTBuilder *builder, const char *subject);
```

This method sets the “sub” attribute, and SDK provides some fixed attribute settings, which can be found in the API document.

```c
int JWTBuilder_Sign(
        JWTBuilder *builder,
        DIDURL *keyid,
        const char *storepass)
```

This method signs JWTBuilder content and generates JWS string.

Keyid is the Authentication Key in DID Document that created JWTBuilder.

Keyid is the DID Document that created JWTBuilder.

```c
const char *JWTBuilder_Compact(JWTBuilder *builder);
```

This method encapsulates JWTBuilder content and generates JWT string. **Attention:** the return value string needs to be released after being used.

```c
int JWTBuilder_Reset(JWTBuilder *builder);
```

Reset the header and body, except “alg,” “kid,” and “iss.”
