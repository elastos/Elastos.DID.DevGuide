# Create jwt

JWT内容通过JWTBuilder来编译，JWTBuilder分为JWTHeader和Claims两个部分，分别填充，最后根据是否签名封装，得到JWT或者JWS Token String。

JWT content is compiled by JWTBuilder. JWTBuilder is divided into two parts, JWTHeader and Claims, which are filled separately. Finally, JWT or JWS Token String is obtained according to whether they are encapsulated and signed or not.

## Example

```typescript
//create jwt header
let h = JWTBuilder.createHeader();
h.setType(JWTHeader.JWT_TYPE)
	.setContentType("json");
h.put("library", "Elastos DID");
h.put("version", "1.0");

let cal =  dayjs();
let iat = cal.unix();
let nbf = cal.add(-1, 'month').unix();
let exp = cal.add(4, 'month').unix();

//create jwt claims
let b = JWTBuilder.createClaims();
b.setSubject("JwtTest")
	.setId("0")
	.setAudience("Test cases")
	.setIssuedAt(iat)
	.setExpiration(exp)
	.setNotBefore(nbf)
	.put("foo", "bar");

//get jwt token
let token = doc.jwtBuilder()
	.setHeader(h)
	.setClaims(b)
	.compact();

if (token)
	console.log("create jwt token successfully.");
else
	console.log("create jwt token failed.");
```

## Usage

```typescript
public jwtBuilder(): JWTBuilder；
```

为了更好的理解，把该方法放到JWT模块中介绍。DID Document提供了方法创建JWTBuilder，得到JWT编译模式。

For better understanding, this method is introduced in the JWT module. The DID document offers a method to create JWTBuilder and get the compiled mode of JWT.

```typescript
public static createHeader(): JWTHeader；
```

该方法创建空的JWTHeader，添加各元素可参考API文档。

This method creates an empty JWTHeader, and the API document can be referred to for adding elements.

```typescript
public static createClaims(): Claims；
```

该方法创建空的Claims，添加各元素可参考API文档。

This method creates empty claims, and the API document can be referred to for adding elements.

```typescript
public setHeader(header: JWTHeader): JWTBuilder；
```

该方法提供将header填充到JWTBuilder中。

This method fills header into JWTBuilder.

```typescript
public setClaims(claims: Claims): JWTBuilder；
```

该方法提供将claims填充到JWTBuilder中。

This method fills claims into JWTBuilder.

```typescript
public async sign(
	password: string,
	keyid: string = null
): Promise<string>；
```

该方法对JWTBuilder内容签名，生成JWS字符串。

This method signs the content of JWTBuilder and generates JWS string.

keyid为创建JWTBuilder的DID Document里的Authentication Key。

Keyid is the authentication key in the DID document that creates JWTBuilder.

keyid为创建JWTBuilder的DID Document。

Keyid is the DID document that creates JWTBuilder.

```typescript
public compact(): string；
```

该方法对JWTBuilder内容封装，生成JWT字符串。

This method encapsulates the content in JWTBuilder and generates JWT string.
