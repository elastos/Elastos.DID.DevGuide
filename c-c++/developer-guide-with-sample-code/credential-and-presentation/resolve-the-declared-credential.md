# Resolve the declared credential

Resolve Credential和Resolve DID有所不同：Resolve DID只需要提供指定DID即可获取链上最新内容；而Resolve Credential可能会因为有无提供`issuer`而出现不同结果。具体情况分析如下：

* 凭证未被进行过任何操作，无论请求是否有`issuer`参数，返回不存在状态，结果不包含任何交易。
* 凭证仅被声明过，无论请求是否有`issuer`参数，返回已声明状态，结果包含一个声明交易。
* 凭证被声明过，且被凭证所有者或者颁发者撤销，无论请求是否有`issuer`参数，返回已撤销状态，结果包含两个交易，一个是声明交易，另一个是所有者或者颁发者撤销的有效交易。
* 凭证未被声明过，被所有者撤销，无论请求是否有`issuer`参数，返回已撤销状态，结果包含一个所有者撤销的有效交易。
* 凭证未被声明过，被凭证颁发者撤销，如果请求有`issuer`参数，返回已撤销状态，结果包含一个颁发者撤销的有效交易；如果请求无`issuer`参数，返回不存在状态，结果不包含任何交易。

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

`id`为需要resolve的凭证id；`status`表明Credential的状态，若接口返回NULL，可以通过status得到是发生错误还是Credential没有被declared过；`force`表明是从链上或者可以从有效期内的缓存中获取凭证。

```c
int Credential_ResolveRevocation(DIDURL *id, DID *issuer);
```

该方法用来检查Credential是否被指定的DID撤销。一般用于第三方kyc凭证查询是否存在由issuer撤销的有效交易。

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

```c
void CredentialBiography_Destroy(CredentialBiography *biography);
```

用完CredentialBiography，将其销毁。
