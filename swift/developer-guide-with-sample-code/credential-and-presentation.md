---
description: >-
  凭证（Credential），是某一实体作出的声明集合，可验证凭证（Verifiable
  Credential）一种特殊的凭证，这是一种防篡改、可由密码学方式证明的凭证。
---

# Credential and presentation

Verifiable Credential 文档：

```
"verifiableCredential" : [ {
    "id" : "#profile",
    "type" : [ "SelfProclaimedCredential", "BasicProfileCredential" ],
    "issuanceDate" : "2021-01-18T14:47:03Z",
    "expirationDate" : "2026-01-18T14:47:03Z",
    "credentialSubject" : {
      "id" : "did:elastos:imUUPBfrZ1yZx6nWXe6LNN59VeX2E6PPKj",
      "name" : "Test Issuer",
      "nation" : "Singapore",
      "language" : "English",
      "email" : "issuer@example.com"
    },
    "proof" : {
      "verificationMethod" : "#primary",
      "signature" : "Bjy3pKeedU2jNp72FUvk9kK39gp5YUy9aK5QtX-AyB3RtDR7YHqlohXov5ZoyfBb3x5tx0zWxL5--PW-pWLk8g"
    }
  } ]
```

Verifiable Credential 提供了很多方法，这里仅做个别展示：

```
// 验证文档
// Verifiable Credential 实例 可以通过DIDDocument的提供的方法，也可以通过传入相关文档生成。
// 从DIDDocument的获得Verifiable Credential实例，这里不做介绍，具体可以查看DIDDocument中相关方法。
// 下面展示一下从外部导入文档生成Verifiable Credential实例
let dict = ["id":"did:elastos:insTmxdDDuS9wHHfeYD1h5C2onEHh3D8Vq#avatar","type":["BasicProfileCredential","SelfProclaimedCredential"],"issuer":"did:elastos:insTmxdDDuS9wHHfeYD1h5C2onEHh3D8Vq","issuanceDate":"2021-07-23T02:06:35Z","expirationDate":"2026-07-23T02:00:00Z","credentialSubject":["id":"did:elastos:insTmxdDDuS9wHHfeYD1h5C2onEHh3D8Vq","avatar":["content-type":"image/png","data":"hive://did:elastos:insTmxdDDuS9wHHfeYD1h5C2onEHh3D8Vq@did:elastos:ig1nqyyJhwTctdLyDFbZomSbZSjyMN1uor/getMainIdentityAvatar1627005986596?params=[\"empty\":0]","type":"elastoshive"]],"proof":["type":"ECDSAsecp256r1","verificationMethod":"did:elastos:insTmxdDDuS9wHHfeYD1h5C2onEHh3D8Vq#primary","signature":"dU6H87DckCOxMNRv1daCyxpNP-jMSwaomPhp_tByB649t3Pmz1cFUf9i0Z7_TSzgfsA2I6dJFCKSDL69jqfjIQ"]] as [String : Any]
let vc = try VerifiableCredential.fromJson(for: dict)

// 从链上直接resolveVerifiable Credential
let id = DIDURL("did:elastos:insTmxdDDuS9wHHfeYD1h5C2onEHh3D8Vq#avatar")
let resolved = try VerifiableCredential.resolve(id)
  
// 验证此DIDDocument是否有效等：
// 文档是否过期
try vc.isExpired()

// 文档是否是原版，是否被篡改
try vc.isGenuine()

// 文档是否有效
try vc.isValid()
```

撤销凭证：

```


```





