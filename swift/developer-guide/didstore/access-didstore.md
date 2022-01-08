# Access DIDStore

DIDStore provides a series of accessor methods for stored objects, which can be divided into the following categories:

* The store method saves the DID object
* The load method reads the DID object
* The contains method tests whether the DID object is stored in the store
* The list method lists specific DID object set
* The delete method deletes the DID object
* Synchronize IDChain data

In general, only the document and privatekey of DID, as well as the associated verifiable credentials, are kept in the DIDStore. The DID document objects of others that can be parsed and obtained from the ID side chain do not need to be saved in DIDStore.

## Relevant Operations of DID

**Store DIDDocument**

```
//创建助记词
let mnemonic = try Mnemonic.generate(Mnemonic.DID_ENGLISH)
// 创建identity
let identity = try RootIdentity.create(mnemonic, "YOUR-MNEMONIC-PASSWORD", true, store, "YOUR-STORE-PASSWORD")
var document = try identity.newDid("YOUR-STORE-PASSWORD")
... // 对document进行编辑处理
try store.storeDid(using: document)
```

**Load DIDDocument**

```
// 例："did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y"
let subject = try DID("YOUR-DIDSTRING") 
let doc = try store.loadDid(subject)
```

**Query whether the specified DID document exists locally**

```
// 例："did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y"
let did = try DID("YOUR-SPECIFIC-DIDSTRING") 
let doc = try store.containsDid(did)
```

**Delete Specified DID**

```
// 例："did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y"
let did = try DID("YOUR-DIDSTRING") 
// result为Bool类型， true删除成功，false删除失败
let result = try store.deleteDid(did)
```

## Relevant Operations of Verifiable Credentials

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

#### Load Credential

```
// 例："did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y#primary"
let didUrl = try DIDURL("YOUR-DIDSTRING")
let vc = try store.loadCredential(byId: didUrl)
```

#### Delete Credential

```
// 例："did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y#primary"
let didUrl = try DIDURL("YOUR-DIDSTRING")
let deleted = store.deleteCredential(didUrl)
```

## Relevant Operations of RootIdentity <a href="#access-rootidentity" id="access-rootidentity"></a>

## Relevant Operations of PrivateKey

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

## Operation Related to Modifying Locally Stored Password

Operation related to modifying locally stored password

```
try store.changePassword("oldStorePassword", "newStorepasswd")
```

Synchronize data on the chain to a local data store:

## Relevant Operations of IDChain Data Synchronization

```
// 使用默认ConflictHandle
try store.synchronize()
```

...
