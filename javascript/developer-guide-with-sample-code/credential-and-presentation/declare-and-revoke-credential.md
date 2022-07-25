# Declare and revoke credential

The Elastos ID chain supports publishing the credential to the chain, which is used for the DID credential information to be declared. Besides, whether the credential has been declared or not, it can be revoked to publicly inform that the credential is invalid and can no longer be trusted.

## Example

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

The credential provides a method to publish the current credential to the chain (i.e. declaration). It should be noted that all credentials that have not been declared or revoked by the owner or issuer can be declared, and the same credential can only be declared once. The declaration can only be initiated by the owner of verifiable credentials.

signKey is the key of the declarant, that is, the owner of the verifiable credential, which is used to sign the published content of the declared credential; storepass refers to the storepass of the DID store that contains the private key used by the declarant for signature.

```typescript
public async revoke(
	signKey: DIDURL | string,
	signer: DIDDocument = null,
	storepass: string = null,
	adapter: DIDTransactionAdapter = null
): void;
```

Credential provides a method to revoke credentials. It should be noted that whether the credential has been declared or not, it can be revoked, but if the credential has been revoked by its owner or issuer, it cannot be revoked again. To revoke the verifiable credential, the credential owner or issuer needs to initiate the corresponding transaction.

SignKey is the key of the revoker, that is, the owner or issuer of the verifiable credential; signer denotes the DID document of the revoker who is the owner of signKey by default.
