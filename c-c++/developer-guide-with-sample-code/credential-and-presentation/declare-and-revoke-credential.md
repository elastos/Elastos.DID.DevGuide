# Declare and revoke credential

Elastos ID链支持Credential上链，用于需要公开声明的DID凭证信息。无论凭证是否被声明过，都可以撤销凭证，以便公开告知该凭证已失效，不可再信任。

Elastos ID chain supports Credential cochain, which is used for DID credential information that needs to be publicly declared. Whether the credential has been declared or not, it can be revoked so as to publicly inform that the credential is invalid and can no longer be trusted.

## Example

声明凭证示例：

Example of declaring credential:

```c
Issuer *issuer = Issuer_Create(issuerid, NULL, store);
DIDURL *credid = DIDURL_NewFromDid(did, "kyccredential");

const char *types[] = {"BasicProfileCredential", "PhoneCredential"};
Property props[4];
props[0].key = "name";
props[0].value = "John";
props[1].key = "gender";
props[1].value = "Male";
props[2].key = "nation";
props[2].value = "Singapore";
props[3].key = "language";
props[3].value = "English";

Credential *vc = Issuer_CreateCredential(issuer, did, credid, types, 2, props, 4, expires, storepass);
//declare credential
int rc = Credential_Declare(vc, NULL, storepass);
if (rc == -1) {
  printf("declare credential error.");
} else if (rc == 0) {
  printf("declare credential failed.");
} else {
  if (Credential_WasDeclared(credid) == 1)
    printf("declare credential successfully.");
}
```

撤销凭证示例：

Example of revoking credential:

```c
//declare by owner, signKey is the one key of owner.
DIDURL *id = Credential_GetId(credential);
int rc = Credential_Declare(credential, signKey, storePass);
if (rc == 1) {
  int status;
  //resolve credential
  Credential *resolved = Credential_Resolve(id, &status, true);
  if (vc)
    printf("declare credential successfully.\n");
}

if (Credential_IsRevoked(resolved) == 1)
	printf("credential is revoked.");
else
	console.log("credential isn't revoked.");
... ... ... ...  
//revoke credential by owner
await credential.revoke(signKey, null, storePass);
```

## Usage

```c
int Credential_Declare(
        Credential *credential,
        DIDURL *signkey,
        const char *storepass);
```

Credential提供将当前凭证发布到链上（即声明）的方法。这里需要说明的是：没有被声明过或者没有被凭证所有者（owner）或者颁发者（issuer）撤销过的凭证都可以被声明。同一个凭证只有一次声明操作。声明操作只能是Verifiable Credential所有者发起。

Credential provides a method to publish the current credential to the chain (that is, declare it). It should be noted here that credentials that have not been declared or revoked by the owner or issuer can be declared. There is only one declaration for one credential. The declaration can only be initiated by the owner of Verifiable Credential.

`signkey`为声明者即Verifiable Credential所有者的Key，用于签名被声明凭证的发布内容，若signkey == NULL，则默认使用所有者的Default Key；storepass为声明者签名所用私钥所在的DID Store的store pass。

_signkey_ is the key of the declarant, that is, the owner of the Verifiable Credential, which is used to sign the published content of the declared credential. If signkey == NULL, the owner’s Default Key is used by default; _storepass_ is the store pass of the DID Store where the private key used by the declarant is signed.

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, declare credential failed;
  ```
* ```
   return value = 1, declare credential successfully.
  ```

```c
int Credential_Revoke(
        Credential *credential,
        DIDURL *signkey,
        const char *storepass);
```

Credential提供撤销凭证的方法。这里需要说明的是：凭证无论是否被声明过，都可以被撤销，但是如果已经被凭证所有者或者颁发者撤销的凭证无法再次被撤销。撤销Verifiable Credential需要通过凭证所有者或者凭证颁发者发起交易来实现。

Credential provides a method to revoke credentials. It should be noted here that a credential can be revoked whether it has been declared or not, but if it has been revoked by the owner or issuer, it cannot be revoked again. Revocation of Verifiable Credential needs to be realized by the transaction initiated by the owner or the issuer.

`signkey`为撤销者即Verifiable Credential所有者或者颁发者的Key，，若signkey == NULL，则默认使用所有者的Default Key。

_signkey_ is the key of the revoker, that is, the owner or issuer of Verifiable Credential. If signkey == NULL, the Default Key of the owner will be used by default.

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, revoke credential failed;
  ```
* ```
   return value = 1, revoke credential successfully.
  ```

```c
int Credential_RevokeById(
        DIDURL *id,
        DIDDocument *document,
        DIDURL *signkey,
        const char *storepass);
```

该方法是根据Credential Id来撤销指定凭证，而不需要获取凭证本身。

This method revokes the specified credential according to the Credential Id without obtaining the credential itself.

`document`为发起revoke的DID Document，`signkey`为该发起者用来签名交易的Authentication Key。

_document_ is the DID Document that initiated the revoke, and _signkey_ is the Authentication Key used by the initiator to sign the transaction.

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, revoke credential failed;
  ```
* ```
   return value = 1, revoke credential successfully.
  ```
