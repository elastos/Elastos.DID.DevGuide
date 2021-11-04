---
description: 以下介绍部分使用DIDStore的方法
---

# Access DIDStore

存储document：

```
//创建助记词
let mnemonic = try Mnemonic.generate(Mnemonic.DID_ENGLISH)
// 创建identity
let identity = try RootIdentity.create(mnemonic, "YOUR-MNEMONIC-PASSWORD", true, store, "YOUR-STORE-PASSWORD")
var document = try identity.newDid("YOUR-STORE-PASSWORD")
... // 对document进行编辑处理
try store.storeDid(using: document)
```

加载document：

```
// 例："did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y"
let subject = try DID("YOUR-DIDSTRING") 
let doc = try store.loadDid(subject)
```

查询本地是否存在指定DID的document：

```
// 例："did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y"
let did = try DID("YOUR-SPECIFIC-DIDSTRING") 
let doc = try store.containsDid(did)
```

删除指定的DID：

```
// 例："did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y"
let did = try DID("YOUR-DIDSTRING") 
// result为Bool类型， true删除成功，false删除失败
let result = try store.deleteDid(did)
```

添加Credential：

```
// 1.创建VerifiableCredential
let documentBuilder = try document.editing()
let selfIssuer = try VerifiableCredentialIssuer(document)
let credentialBuilder = selfIssuer.editingVerifiableCredentialFor(did: document.subject)
let properties: [String: String] = ["name": "John", "language": "English","email": "john@example.com" ]
let vcProfile = try credentialBuilder.withId("#profile")
                .withTypes("BasicProfileCredential", "SelfProclaimedCredential")
                .withProperties(properties)
                .seal(using: storePassword)
// 2.添加到document
try documentBuilder.appendCredential(with: vcProfile)
document = try documentBuilder.seal(using: "YOUR-STORE-PASSWORD")
// 3.持久化到本地 
try store.storeDid(using: document)
```

加载Credential：

```
// 例："did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y#primary"
let didUrl = try DIDURL("YOUR-DIDSTRING")
let vc = try store.loadCredential(byId: didUrl)
```

删除Credential：

```
// 例："did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y#primary"
let didUrl = try DIDURL("YOUR-DIDSTRING")
let deleted = store.deleteCredential(didUrl)
```

修改本地存储密码：

```
try store.changePassword("oldStorePassword", "newStorepasswd")
```

同步链上的数据到本地：

```
// 使用默认ConflictHandle
try store.synchronize()
```

...



