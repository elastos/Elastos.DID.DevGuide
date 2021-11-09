# Package the credentials as presentation for the selective disclosure request

可验证表达是指包含实体可验证凭证子集及副署签名(countersign)的数据集合，用于对第三方表明自身身份。

可验证表达也可不含可验证凭证，为一个空的表达实体。

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

did是Presentation的持有者；signKey是Presentation的持有者用来签名封装Presentation的Authentication Key。

```typescript
public credentials(...credentials: VerifiableCredential[]): Builder;
```
该方法用于添加VerifiableCredential。

```typescript
public realm(realm: string): Builder;
```
VerifiablePresentation.Builder提供设置realm的方法，表明该presentation适用的领域和地址。

```typescript
public nonce(nonce: string): Builder;
```
VerifiablePresentation.Builder提供设置nonce的方法，是签名操作使用的随机值。

```typescript
public async seal(storepass: string): Promise<VerifiablePresentation>;
```
VerifiablePresentation.Builder提供封装方法，最后得到VerifiablePresentation。
