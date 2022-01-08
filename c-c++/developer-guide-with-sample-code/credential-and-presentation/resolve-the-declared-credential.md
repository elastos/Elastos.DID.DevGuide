# Resolve the Declared Credential

Resolve Credential is different from Resolve DID. Resolve DID only needs to provide the specified DID to get the latest content on the chain, where the Resolve Credential may have different results depending on whether issuer is provided or not. The specific situation analysis is as follows:

* The credential has not been operated. Whether the request has the issuer parameter or not, the status of non-existence is returned, and the result does not contain any transaction.
* The credential has only been declared. Whether the request has the issuer parameter or not, the declared status is returned, and the result contains a declared transaction.
* The credential has been declared and revoked by the owner or issuer of the credential. Whether the request has the issuer parameter or not, the revoked status is returned. The result includes two transactions, one is the declared transaction, and the other is the valid transaction revoked by the owner or issuer.
* The credential is undeclared and revoked by the owner. Whether the request has the issuer parameter or not, the revoked status is returned, and the result contains a valid transaction revoked by the owner.
* The credential is undeclared and revoked by the issuer. If the request has the issuer parameter, the revoked status will be returned, and the result contains a valid transaction revoked by the issuer. If the request has no issuer parameter, it will return the non-existent status, and the result will not contain any transactions.

## Example

```c
//declare by owner, 'signkey' is the key of owner
DIDURL *id = Credential_GetId(vc);
int rc = Credential_Declare(vc, signkey, storePass);

//resolve credential
int status = 0;
Credential *resolved = Credential_Resolve(id, &status, true);
if (resolved) {
	console.log("credential is declared and resolved successfully.");
  Credential_Destroy(resolved);
} else {
	console.log("credential is resolved failed.");
}
... ... ... ...  
//revoke credential by owner
Credential_Revoke(vc, signKey, NULL, storePass);
CredentialBiography *bio = Credential_ResolveBiography(id, Credential_GetIssuer(vc));
//get information from CredentialBiography
... ... ... ...
CredentialBiography_Destroy(bio);
```

## Usage

```c
Credential *Credential_Resolve(DIDURL *id, int *status, bool force);
```

The SDK provides a method to resolve the credential, where only the declared credential can be resolved.

id the credential ID that needs to be resolved; status indicates the status of Credential. If the interface returns NULL, you can get whether there is an error or credential has not been declared by status; force indicates that credentials can be obtained from the chain or from the cache within the effective period.

```c
int Credential_ResolveRevocation(DIDURL *id, DID *issuer);
```

This method is used to check whether Credential has been revoked by the specified DID. Generally, it's used to inquire whether there is a valid transaction revoked by the issuer with the third-party KYC credential.

* ```
   return value = -1, if error occurs；
  ```
* ```
   return value = 0, credential isn't revoked by issuer；
  ```
* ```
   return value = 1, credential is revoked by issuer。
  ```

```c
CredentialBiography *Credential_ResolveBiography(
        DIDURL *id, DID *issuer);
```

This method is used to resolve all the transaction contents of the credential, and the returned CredentialBiography object. issuer is the issuer of the credential, which can be NULL. If no issuer is provided, the transaction of issuer revoke will be ignored. CredentialBiography provides methods to obtain the status of credentials on the chain, the number of transactions, the specific transaction content, etc. Refer to API documentation for specific methods.

```c
void CredentialBiography_Destroy(CredentialBiography *biography);
```

The CredentialBiography needs to be destroyed after being used.
