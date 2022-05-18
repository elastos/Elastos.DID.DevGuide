# Create JWT

JWT content is compiled by JWTBuilder, which is divided into two parts: JWTHeader and Claims, which are filled separately. Finally, JWT or JWS Token String is obtained according to whether they are encapsulated and signed or not.

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

For better understanding, this method is introduced in the JWT module. The DID document offers a method to create JWTBuilder and get the compiled mode of JWT.

```typescript
public static createHeader(): JWTHeader；
```

This method creates an empty JWTHeader, and the API document can be referred to for adding elements.

```typescript
public static createClaims(): Claims；
```

This method creates empty claims, and the API document can be referred to for adding elements.

```typescript
public setHeader(header: JWTHeader): JWTBuilder；
```

This method fills header into JWTBuilder.

```typescript
public setClaims(claims: Claims): JWTBuilder；
```

This method fills claims into JWTBuilder.

```typescript
public async sign(
	password: string,
	keyid: string = null
): Promise<string>；
```

This method signs the content of JWTBuilder and generates JWT string.

Keyid is the authentication key in the DID document that creates JWTBuilder.

Keyid is the DID document that creates JWTBuilder.

```typescript
public compact(): string；
```

This method encapsulates the content in JWTBuilder and generates JWT string.
