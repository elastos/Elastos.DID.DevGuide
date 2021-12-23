# Access DIDStore

DIDStore 提供了针对所存储对象的一系列读写方法，这些方法分为一下几类：

DID Store provides a series of reading and writing methods for stored objects, which can be divided into the following categories:

* store 系列方法保存 DID 对象
* load 系列方法读取 DID 对象
* contains 系列方法测试 DID 对象是否保存在 store 中
* list 系列方法列表特定 DID 对象集
* delete 系列方法删除 DID 对象
* The store series method saves the DID object
* The load series method reads the DID object
* The contains series method tests whether the DID object is stored in the store
* List series method list specific DID object set
* The delete series method deletes the DID object

一般而言，DIDStore 中仅保存自己持有的 DID 的 document 和 privatekey，以及相关联的可验证凭证等。可以从 ID 侧链上解析获取的他人的 DID 文档对象无需保存到 DIDStore 中。

Ordinarily, only the document and Private Key of personal DID, as well as the associated verifiable credentials, are kept in the DID Store. DID Document objects of others can be parsed and obtained from the ID side chain and do not need to be saved in DID Store.

## DID 的相关操作

Related operation of DID

```java
DIDStore store; // an opened store instance
DIDDocument doc; // a DIDDoucment instance

// Save the DID document to the store
store.storeDid(doc);

// Load the DID document from the store
DIDDocument myDoc = store.loadDid("did:elastos:ihQQhyf6PW4YEc8mRi8AWmitCcMxz5kiBq");

// Check the store has this DID document
boolean exists = store.containsDid("did:elastos:ihQQhyf6PW4YEc8mRi8AWmitCcMxz5kiBq");

// Check the store has any DIDs exist
boolean exists = store.containsDid();

// Delete the specific DID
store.deleteDid("did:elastos:ihQQhyf6PW4YEc8mRi8AWmitCcMxz5kiBq");

// List all DIDs in the store
List<DID> dids = store.listDids();
// Or list DIDs with a customized filter
List<DID> dids = store.listDids((did) -> {
        // return true if include this DID, otherwise return false.
        return true;
    });
```

## 可验证凭证相关操作

Related operation of verifiable credentials

```java
DIDStore store; // an opened store instance
VerifiableCredential vc; // a VerifiableCredential instance

// Save the credential to the store
store.storeCredential(vc);

// Load the credential from the store
VerifiableCredential myVc = store.loadCredential(
    "did:elastos:ihQQhyf6PW4YEc8mRi8AWmitCcMxz5kiBq#profile");

// Check the store has this credential
boolean exists = store.containsCredential(
    "did:elastos:ihQQhyf6PW4YEc8mRi8AWmitCcMxz5kiBq#profile");

// Check the store has any credential exists for specific DID
boolean exists = store.containsCredentials(
    "did:elastos:ihQQhyf6PW4YEc8mRi8AWmitCcMxz5kiBq");

// Delete the specific credential
store.deleteCredential(
    "did:elastos:ihQQhyf6PW4YEc8mRi8AWmitCcMxz5kiBq#profile");

// List all credentials for specific DID in the store
List<DIDURL> vcIds = store.listCredentials(
    "did:elastos:ihQQhyf6PW4YEc8mRi8AWmitCcMxz5kiBq");
// Or list credenitals with a customized filter
List<DIDURL> vcIds = store.listCredentials(
    "did:elastos:ihQQhyf6PW4YEc8mRi8AWmitCcMxz5kiBq", (vcId) -> {
        // return true if include this credential, otherwise return false.
        return true;
    });
```

## RootIdentity 相关操作 <a href="#access-rootidentity" id="access-rootidentity"></a>

Related operations of Root Identity

## PrivateKey 相关操作

Related operations of Private Key

```java
DIDStore store; // an opened store instance
byte[] privateKey; // the plain binary private key
DIDURL keyId = new DIDURL("did:elastos:ihQQhyf6PW4YEc8mRi8AWmitCcMxz5kiBq#official");
String storePasswd = "secret"; // should be your store password

// Save the private key to the store
store.storePrivateKey(keyId, privateKey, storePasswd);

// Check the private key exists in the store
boolean exists = store.containsPrivateKey(keyId);

// Delete the specific private key
store.deletePrivateKey(keyId);
```
