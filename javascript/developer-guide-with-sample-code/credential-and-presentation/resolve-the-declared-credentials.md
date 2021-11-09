# Resolve the declared credential

Resolve Credential和Resolve DID有所不同：Resolve DID只需要提供指定DID即可获取链上最新内容；而Resolve Credential可能会因为有无提供`issuer`而出现不同结果。具体情况分析如下：

-   凭证未被进行过任何操作，无论请求是否有`issuer`参数，返回不存在状态，结果不包含任何交易。
-   凭证仅被声明过，无论请求是否有`issuer`参数，返回已声明状态，结果包含一个声明交易。
-   凭证被声明过，且被凭证所有者或者颁发者撤销，无论请求是否有`issuer`参数，返回已撤销状态，结果包含两个交易，一个是声明交易，另一个是所有者或者颁发者撤销的有效交易。
-   凭证未被声明过，被所有者撤销，无论请求是否有`issuer`参数，返回已撤销状态，结果包含一个所有者撤销的有效交易。
-   凭证未被声明过，被凭证颁发者撤销，如果请求有`issuer`参数，返回已撤销状态，结果包含一个颁发者撤销的有效交易；如果请求无`issuer`参数，返回不存在状态，结果不包含任何交易。

## Example

```typescript
//declare by owner
await credential.declare(signKey, storePass);
let id = credential.getId();
//resolve credential
let resolved = await VerifiableCredential.resolve(id);
if (await resolved.isRevoked())
	console.log("credential is revoked.");
else
	console.log("credential isn't revoked.");
... ... ... ...  
//revoke credential by owner
await credential.revoke(signKey, null, storePass);
let bio = await VerifiableCredential.resolveBiography(id, credential.getIssuer());
//get information from CredentialBiography
... ... ... ...
```

## Usage

```typescript
public static async resolve(
	id: DIDURL | string,
	issuer: DID | string = null,
	force = false
): Promise<VerifiableCredential>;
```
SDK提供方法用来resolve凭证，只有被declare过的凭证才能被resolve到。

id为需要resolve的凭证id；issuer表明该credential的颁发者，主要用于第三方kyc凭证查询是否存在由issuer撤销的有效交易。如果无该字段，解析结果不包含issuer撤销的交易。

force表明是否去链上resolve还是允许从有效期内的cache中获取凭证。

```typescript
public static resolveBiography(
	id: DIDURL,
	issuer: DID
): Promise<CredentialBiography>;
```
SDK提供resolve指定凭证所有交易记录的方法。

id为凭证的id标识；issuer为凭证的颁发者，返回CredentialBiography object，CredentialBiography提供方法获取凭证在链上的状态和所有交易记录。具体方法可查阅API文档。
