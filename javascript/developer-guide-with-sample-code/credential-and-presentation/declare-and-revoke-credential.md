# Declare and revoke credential

Elastos ID链支持Credential上链，用于需要公开声明的DID凭证信息。同时，无论凭证是否被声明过，都可以撤销凭证，以便公开告知该凭证已失效，不可再信任。

## Example

声明凭证示例：
```typescript
let vc =  await selfIssuer.issueFor(did)
	.id("#test")
	.type("SelfProclaimedCredential")
	.properties({"name": "littlefish"})
	.seal(storePass);

vc.getMetadata().attachStore(doc.getStore());
//declare self-claimed credential
await vc.declare(null, storePass);
if (await vc.wasDeclared())
	console.log("declare credential successfully.");
else
	console.log("declare credential failed.");
```

撤销凭证示例：
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
```

## Usage

```typescript
public async declare(
	signKey: DIDURL,
	storepass: string,
	adapter: DIDTransactionAdapter = null
): Promise<void>;
```
Credential提供将当前凭证发布到链上（即声明）的方法。这里需要说明的是：没有被声明过或者没有被凭证所有者（owner）或者颁发者（issuer）撤销过的凭证都可以被声明。同一个凭证只有一次声明操作。声明操作只能是Verifiable Credentials所有者发起。

signKey为声明者即Verifiable Credential所有者的Key，用于签名被声明凭证的发布内容；storepass为声明者签名所用私钥所在的DID Store的store pass。

```typescript
public async revoke(
	signKey: DIDURL | string,
	signer: DIDDocument = null,
	storepass: string = null,
	adapter: DIDTransactionAdapter = null
): void;
```
Credential提供撤销凭证的方法。这里需要说明的是：凭证无论是否被声明过，都可以被撤销，但是如果已经被凭证所有者或者颁发者撤销的凭证无法再次被撤销。撤销Verifiable Credential需要通过凭证所有者或者凭证颁发者发起交易来实现。

signKey为撤销者即Verifiable Credential所有者或者颁发者的Key；signer为撤销凭证者的DID Document，默认为signKey的所有者。
