---
description: 基于区块链建立的符合W3C标准的数字身份系统，DID是用户的数字身份，格式如：did-METHOD-SPECIFIC-ID
---

# DID

## DID Definition <a href="did-definition" id="did-definition"></a>

```
// did-METHOD-SPECIFIC-ID,例如：
"did:elastos:imUUPBfrZ1yZx6nWXe6LNN59VeX2E6PPKj"
```

## DID Fragment <a href="did-fragment" id="did-fragment"></a>

```
#profile：指定引用哪个publicKey
#recovery：指定publicKey的作用
#key：
```

## DID Document Definition <a href="did-document-definition" id="did-document-definition"></a>

设计 DID Document 如下格式：

```
{
  "id{
	"id": "did:elastos:foobar",
	"controller": ["did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y", "did:elastos:idwuEMccSpsTH4ZqrhuHqg6y8XMVQAsY5g", "did:elastos:igXiyCJEUjGJV1DMsMa4EbWunQqVg97GcS"],
	"multisig": "2:3",
	"publicKey": [{
		"id": "#key2",
		"publicKeyBase58": "mhSB8BgVRwVUXvGvsUEnu7mfNawxEVwYtM1MmfCbtENS"
	}, {
		"id": "#key3",
		"publicKeyBase58": "fjVZ5bayjN1fxp4qAes6aRqKdJiDheKq9yF7wjHR37fv"
	}],
	"authentication": ["#key2", "#key3"],
	"verifiableCredential": [{
		"id": "#email",
		"type": ["BasicProfileCredential", "EmailCredential", "InternetAccountCredential"],
		"issuer": "did:elastos:example",
		"issuanceDate": "2021-07-13T04:35:57Z",
		"expirationDate": "2026-07-13T04:35:57Z",
		"credentialSubject": {
			"id": "did:elastos:foobar",
			"email": "foobar@example.com"
		},
		"proof": {
			"created": "2021-07-13T04:35:57Z",
			"verificationMethod": "did:elastos:imUUPBfrZ1yZx6nWXe6LNN59VeX2E6PPKj#primary",
			"signature": "vYYGCC260ZsQt-XFOnyT05c14ks3plm6mIfz5OZLf7UyPxh0mrVVfFmrpiRMqW47aLk7Jb4Ygerlv9qNkVLnHQ"
		}
	}, {
		"id": "#profile",
		"type": ["BasicProfileCredential", "SelfProclaimedCredential"],
		"issuanceDate": "2021-07-13T04:35:57Z",
		"expirationDate": "2026-07-13T04:35:57Z",
		"credentialSubject": {
			"id": "did:elastos:foobar",
			"email": "contact@foobar.com",
			"language": "Chinese",
			"name": "Foo Bar Inc"
		},
		"proof": {
			"created": "2021-07-13T04:35:57Z",
			"verificationMethod": "did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y#primary",
			"signature": "exhyiQ1awewFgxVwv59n3bsvrZ3D8XGeBmPbpAf_F_22tOoXRWfnVBbPOJmlq87QvnTDkmB7MlGidVEZyHf-AQ"
		}
	}],
	"service": [{
		"id": "#vault",
		"type": "Hive.Vault.Service",
		"serviceEndpoint": "https://foobar.com/vault"
	}, {
		"id": "#vcr",
		"type": "CredentialRepositoryService",
		"serviceEndpoint": "https://foobar.com/credentials",
		"ABC": "Helloworld",
		"BAR": "Foobar",
		"DATE": "2021-07-13T04:35:57Z",
		"FOO": 678,
		"FOOBAR": "Lalala...",
		"MAP": {
			"ABC": "Helloworld",
			"BAR": "Foobar",
			"DATE": "2021-07-13T04:35:57Z",
			"FOO": 678,
			"FOOBAR": "Lalala...",
			"abc": "helloworld",
			"bar": "foobar",
			"date": "2021-07-13T04:35:57Z",
			"foo": 123,
			"foobar": "lalala..."
		},
		"abc": "helloworld",
		"bar": "foobar",
		"date": "2021-07-13T04:35:57Z",
		"foo": 123,
		"foobar": "lalala...",
		"map": {
			"ABC": "Helloworld",
			"BAR": "Foobar",
			"DATE": "2021-07-13T04:35:57Z",
			"FOO": 678,
			"FOOBAR": "Lalala...",
			"abc": "helloworld",
			"bar": "foobar",
			"date": "2021-07-13T04:35:57Z",
			"foo": 123,
			"foobar": "lalala..."
		}
	}],
	"expires": "2026-07-13T04:35:56Z",
	"proof": [{
		"created": "2021-07-13T04:35:57Z",
		"creator": "did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y#primary",
		"signatureValue": "MURa9J0ZVevrfo7GFmV6dkE7aS17BIj3SYLLKjTuIQyI97uF9nk4vbpcRf_O1Xt66KJCfl7LIg-gqbJBHeyZqw"
	}, {
		"created": "2021-07-13T04:35:57Z",
		"creator": "did:elastos:igXiyCJEUjGJV1DMsMa4EbWunQqVg97GcS#primary",
		"signatureValue": "pTXRoVyO-O4fiGI3GTFvt1gA_UF4axQZmy6wbHg7zFbFf2l2_CJAsBXvgsn-efCDL5VXZPGIV_auacr_KtkRig"
	}]
}
```

* `publicKey`： 是公钥的列表。
* `authentication`： 说明拥有哪个公钥对应的私钥的用户就是此 DID 的拥有者；通过`#keys`来指定。
* `recovery`： 是可用于恢复的公钥列表；通过`keys`来指定哪个公钥。
* `service`： 一些能够使用此 DID 的 Endpoint，例如这里放了 DIDResolve 服务的地址。

## &#x20;<a href="did-document-definition" id="did-document-definition"></a>





