# Resolve the declared credential

Resolve Credential和Resolve DID有所不同：Resolve DID只需要提供指定DID即可获取链上最新内容；而Resolve Credential可能会因为有无提供`issuer`而出现不同结果。具体情况分析如下：

Resolve Credential is different from Resolve DID: Resolve DID only needs to provide the specified DID to get the latest content on the chain; Resolve Credential may have different results depending on whether issuer is provided or not. The specific situation analysis is as follows:

* 凭证未被进行过任何操作，无论请求是否有`issuer`参数，返回不存在状态，结果不包含任何交易。
* The credential has not been operated. Whether the request has the issuer parameter or not, the status of non-existence is returned, and the result does not contain any transaction.
* 凭证仅被声明过，无论请求是否有`issuer`参数，返回已声明状态，结果包含一个声明交易。
* The credential has only been declared. Whether the request has the issuer parameter or not, the declared status is returned, and the result contains a declared transaction.
* 凭证被声明过，且被凭证所有者或者颁发者撤销，无论请求是否有`issuer`参数，返回已撤销状态，结果包含两个交易，一个是声明交易，另一个是所有者或者颁发者撤销的有效交易。
* The credential has been declared and revoked by the owner or issuer of the credential. Whether the request has the issuer parameter or not, the revoked status is returned. The result includes two transactions, one is the declared transaction, and the other is the valid transaction revoked by the owner or issuer.
* 凭证未被声明过，被所有者撤销，无论请求是否有`issuer`参数，返回已撤销状态，结果包含一个所有者撤销的有效交易。
* The credential is undeclared and revoked by the owner. Whether the request has the issuer parameter or not, the revoked status is returned, and the result contains a valid transaction revoked by the owner.
* 凭证未被声明过，被凭证颁发者撤销，如果请求有`issuer`参数，返回已撤销状态，结果包含一个颁发者撤销的有效交易；如果请求无`issuer`参数，返回不存在状态，结果不包含任何交易。
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

SDK提供方法用来resolve凭证，只有被declare过的凭证才能被resolve到。

SDK provides a method to resolve the credential. Only the declared credential can be resolved.

`id`为需要resolve的凭证id；`status`表明Credential的状态，若接口返回NULL，可以通过status得到是发生错误还是Credential没有被declared过；`force`表明是从链上或者可以从有效期内的缓存中获取凭证。

_id_ is the credential ID that needs to be resolved; _status_ indicates the status of Credential. If the interface returns NULL, you can get whether there is an error or credential has not been declared by status; _force_ indicates that credentials can be obtained from the chain or from the cache within the effective period.

```c
int Credential_ResolveRevocation(DIDURL *id, DID *issuer);
```

该方法用来检查Credential是否被指定的DID撤销。一般用于第三方kyc凭证查询是否存在由issuer撤销的有效交易。

This method is used to check whether Credential has been revoked by the specified DID. Generally, it is used to inquire whether there is a valid transaction revoked by the issuer with the third-party kyc credential.

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

该方法用来resolve凭证的所有交易内容，返回的CredentialBiography object。`issuer`为凭证的颁发者，可为NULL，若不提供issuer则回忽略issuer revoke的交易。CredentialBiography提供方法获取凭证在链上的状态，交易个数，具体交易内容等。具体方法可查阅API文档。

This method is used to resolve all the transaction contents of the credential, and the returned CredentialBiography object. _issuer_ is the issuer of the credential, which can be NULL. If no issuer is provided, the transaction of issuer revoke will be ignored. CredentialBiography provides methods to obtain the status of credentials on the chain, the number of transactions, the specific transaction content, etc. Refer to API documentation for specific methods.

```c
void CredentialBiography_Destroy(CredentialBiography *biography);
```

用完CredentialBiography，将其销毁。

The CredentialBiography needs to be destroyed after using.
