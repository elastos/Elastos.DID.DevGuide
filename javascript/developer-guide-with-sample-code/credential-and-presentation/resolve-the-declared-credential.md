# Resolve the Declared Credential

Resolve credential is different from resolve DID. Resolve DID only requires the specified DID to get the latest content on the chain; resolve credential may return different results, which depends on whether the issuer is provided or not. The specific situation is analyzed as follows:

* If the credential has not been operated, no matter whether the request contains the issuer parameter or not, non-presence status is returned, and the result does not include any transaction.
* If the credential has only been declared, no matter whether the request contains the issuer parameter or not, the declared status is returned, and the result includes a declared transaction.
* If the credential has been declared and revoked by its owner or issuer, no matter whether the request contains the issuer parameter or not, the revoked status is returned. The result includes two transactions, namely the declared transaction and the valid transaction revoked by the owner or issuer.
* If the credential has not been declared or it is revoked by the owner, no matter whether the request contains the issuer parameter or not, the revoked status is returned, and the result includes a valid transaction revoked by the owner.
* If the credential has not been declared or it is revoked by the issuer, no matter whether the request contains the issuer parameter or not, the revoked status is returned, and the result includes a valid transaction revoked by the issuer. If the request contains no issuer parameter, non-presence status is returned, and the result does not include any transaction.

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

The SDK offers a method to resolve credentials, and only declared credentials can be resolved.

id refers to the ID of the credential to be resolved; issuer represents the issuer of the credential, which is mainly used for the third-party KYC credential to inquire whether there is a valid transaction revoked by the issuer. Without this field, the analysis result will not include the transaction revoked by the issuer.

Force denotes whether to resolve the credential on the chain or get the credential from the cache within its validity period.

```typescript
public static resolveBiography(
	id: DIDURL,
	issuer: DID
): Promise<CredentialBiography>;
```

The SDK provides a method to resolve all the transaction records of the specified credential.

id is the identifier of the credential; issuer is the issuer of the credential, and the CredentialBiography object is returned. CredentialBiography provides a method to obtain the status and all transaction records of the credential on the chain. Refer to “API document” for specifics.
