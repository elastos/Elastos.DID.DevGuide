# Access DIDStore
DIDStore 提供了针对所存储对象的一系列读写方法，这些方法分为一下几类：

- store 系列方法保存 DID 对象
- load 系列方法读取 DID 对象
- contains 系列方法测试 DID 对象是否保存在 store 中
- list 系列方法列表特定 DID 对象集
- delete 系列方法删除 DID 对象
- synchronize IDChain数据同步

一般而言，DIDStore 中仅保存自己持有的 DID 的 document 和 privatekey，以及相关联的可验证凭证等。可以从 ID 侧链上解析获取的他人的 DID 文档对象无需保存到 DIDStore 中。

## DID 的相关操作

#### 存储DIDDocument

```
//创建助记词
let mnemonic = try Mnemonic.generate(Mnemonic.DID_ENGLISH)
// 创建identity
let identity = try RootIdentity.create(mnemonic, "YOUR-MNEMONIC-PASSWORD", true, store, "YOUR-STORE-PASSWORD")
var document = try identity.newDid("YOUR-STORE-PASSWORD")
... // 对document进行编辑处理
try store.storeDid(using: document)
```

#### 加载DIDDocument

```
// 例："did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y"
let subject = try DID("YOUR-DIDSTRING") 
let doc = try store.loadDid(subject)
```

#### 查询本地是否存在指定DID的document

```
// 例："did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y"
let did = try DID("YOUR-SPECIFIC-DIDSTRING") 
let doc = try store.containsDid(did)
```

#### 删除指定的DID

```
// 例："did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y"
let did = try DID("YOUR-DIDSTRING") 
// result为Bool类型， true删除成功，false删除失败
let result = try store.deleteDid(did)
```

## 可验证凭证相关操作

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

#### 加载Credential

```
// 例："did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y#primary"
let didUrl = try DIDURL("YOUR-DIDSTRING")
let vc = try store.loadCredential(byId: didUrl)
```

#### 删除Credential

```
// 例："did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y#primary"
let didUrl = try DIDURL("YOUR-DIDSTRING")
let deleted = store.deleteCredential(didUrl)
```

## RootIdentity 相关操作 {#access-rootidentity}


## PrivateKey 相关操作

```
let store: DIDStore = ... // an opened store instance
let privateKey: Data = ... // the data private key
let keyId = try DIDURL("did:elastos:iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2#official")
let storePasswd = "secret" // should be your store password

// Save the private key to the store
try store.storePrivateKey(for: keyId, privateKey: privateKey, using: storePasswd)

// Check the private key exists in the store
let exists = store.containsPrivateKey(for: keyId)

// Delete the specific private key
store.deletePrivateKey(for: keyId)
```

## 修改本地存储密码 相关操作

```
try store.changePassword("oldStorePassword", "newStorepasswd")
```

同步链上的数据到本地：
## IDChain数据同步 相关操作

```
// 使用默认ConflictHandle
try store.synchronize()
```

...


