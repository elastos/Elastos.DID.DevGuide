# Package the credentials as presentation for the selective disclosure request

可验证表达是指包含实体可验证凭证子集及副署签名(countersign)的数据集合，用于对第三方表明自身身份。

Verifiable presentation refers to a data set containing a subset of verifiable credentials and countersign of an entity, which is used to show the identity to a third party.

可验证表达也可不含可验证凭证，为一个空的表达实体。

Verifiable presentation can also be an empty presentation entity rather than include any verifiable credential.

## Example

```typescript
//DID has two 'TwitterCredential' and 'PassportCredential' credentials to package
let pb = await VerifiablePresentation.createFor(doc.getSubject(), null, store);
let vp = await pb
	.credentials(doc.getCredential("#profile"))
	.credentials(doc.getCredential("#email"))
	.credentials(TwitterCredential)
	.credentials(PassportCredential)
	.realm("https://example.com/")
	.nonce("873172f58701a9ee686f0630204fee59")
	.seal(TestConfig.storePass);
// to use presentation
... ... ... ...
```

## Usage

```typescript
public static async createFor(
	did: DID | string,
	signKey: DIDURL | string | null,
	store: DIDStore
): Promise<VerifiablePresentation.Builder>;
```

该方法获取the Builder object用于生成指定DID的Presentation。

This method gets the builder object to generate the presentation of the specified DID.

did是Presentation的持有者；signKey是Presentation的持有者用来签名封装Presentation的Authentication Key。

Did is the owner of the presentation; signKey is the authentication key for the owner of the presentation to sign and encapsulate the presentation.

```typescript
public credentials(...credentials: VerifiableCredential[]): Builder;
```

该方法用于添加VerifiableCredential。

This method applies to adding VerifiableCredential.

```typescript
public realm(realm: string): Builder;
```

VerifiablePresentation.Builder提供设置realm的方法，表明该presentation适用的领域和地址。

VerifiablePresentation.Builder provides a method to set realm, which indicates the applicable domain and address of this presentation.

```typescript
public nonce(nonce: string): Builder;
```

VerifiablePresentation.Builder提供设置nonce的方法，是签名操作使用的随机值。

VerifiablePresentation.Builder provides a method to set nonce, which is a random value used by the sign action.

```typescript
public async seal(storepass: string): Promise<VerifiablePresentation>;
```

VerifiablePresentation.Builder提供封装方法，最后得到VerifiablePresentation。

VerifiablePresentation.Builder provides the encapsulation method, which finally obtains the VerifiablePresentation.
