# Declare and revoke credential

Elastos ID链支持Credential上链，用于需要公开声明的DID凭证信息。同时，无论凭证是否被声明过，都可以撤销凭证，以便公开告知该凭证已失效，不可再信任。

The Elastos ID chain supports publishing the credential to the chain, which is used for the DID credential information to be declared. Besides, whether the credential has been declared or not, it can be revoked to publicly inform that the credential is invalid and can no longer be trusted.

## Example

声明凭证示例：

Example of declaring the credential:

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

Example of revoking the credential:

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

The credential provides a method to publish the current credential to the chain, that is, declaration. It should be noted that all credentials that have not been declared or revoked by the owner or issuer can be declared. The same credential can only be declared once. The declaration can only be initiated by the owner of verifiable credentials.

signKey为声明者即Verifiable Credential所有者的Key，用于签名被声明凭证的发布内容；storepass为声明者签名所用私钥所在的DID Store的store pass。

signKey is the key of the declarant, that is, the owner of the verifiable credential, which is used to sign the published content of the declared credential; storepass refers to the storepass of the DID store that contains the private key used by the declarant for signature.



```typescript
public async revoke(
	signKey: DIDURL | string,
	signer: DIDDocument = null,
	storepass: string = null,
	adapter: DIDTransactionAdapter = null
): void;
```

Credential提供撤销凭证的方法。这里需要说明的是：凭证无论是否被声明过，都可以被撤销，但是如果已经被凭证所有者或者颁发者撤销的凭证无法再次被撤销。撤销Verifiable Credential需要通过凭证所有者或者凭证颁发者发起交易来实现。

Credential provides a method to revoke credentials. It should be noted that whether the credential has been declared or not, it can be revoked, but if the credential has been revoked by its owner or issuer, it cannot be revoked again. To revoke the verifiable credential, the credential owner or issuer need to initiate the corresponding transaction.

signKey为撤销者即Verifiable Credential所有者或者颁发者的Key；signer为撤销凭证者的DID Document，默认为signKey的所有者。

SignKey is the key of the revoker, that is, the owner or issuer of the verifiable credential; signer denotes the DID document of the revoker who is the owner of signKey by default.
