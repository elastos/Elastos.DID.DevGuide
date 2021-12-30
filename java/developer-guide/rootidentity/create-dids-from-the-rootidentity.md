# Create DIDs from the Root Identity

In the Elastos DID SDK, all primitive DIDs are created from Root Identity. Each created DID corresponds to an integer index value, and the index value space is 0 \~ 2,147,483,647. There is a unique correspondence between each index value and the DID it created, that is, the DID created by the index is always consistent.

When creating a DID from Root Identity, DID document and Private Key corresponding to the newly created DID will be saved to the DID Store where Root Identity is located by default. The newly created DID will not be published automatically, but only a local DID object. If it needs to be published, it needs to be published explicitly - see [Publishing DID](../did/publish-did.md).



## 使用默认索引创建 DID

Create DID with default index

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
RootIdentity identity; // a RootIdentity instance

DIDDocument doc = identity.newDid(storePasswd);
```

使用默认索引创建 DID 时，不需要指定索引值，RootIdentity 会使用当前记录的下一个可用索引创建，并且自动更新下一个可用索引。**推荐使用这种方式创建新的 DID**。

When you create DID with the default index, you don’t need to specify an index value. The next available index of the current record will be used by Root Identity, and the next available index will be updated automatically. This method is recommended during the creation of a new DID.

## 指定索引创建 DID

Specify index to create DID

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
RootIdentity identity; // a RootIdentity instance
int index = 8; // the specified index

DIDDocument doc = identity.newDid(index, storePasswd);
```

在指定索引创建 DID 时，指定的索引值不被 RootIdentity 管理，所以需要开发者或者应用来管理可用的索引，并避免重复使用索引引起的冲突。

When the DID is created by specified index, the specified index value is not managed by Root Identity, so developers or applications are needed to manage the available indexes and avoid conflicts caused by repeated use of indexes.

## 重新创建 DID 对象和其文档

Re-create DID objects and their documents

如果需要重新创建已经创建过的 DIDs，默认情况下会因为 DIDs 已经存在而失败，可以使用具有 overwrite 参数的方法创建，如果 overwrite 为 true 的时候，DID SDK 将忽略已经存在的 DID 对象，并重新创建对应的 DID 文档对象，并重新生成其对应的 PrivateKey。

If you need to re-create the created DIDs, it will fail by default because the DIDs already exist. Then you can use the method with overwrite parameter to create. If overwrite is true, DID SDK will ignore the already existing DID object, re-create the corresponding DID document object and regenerate its corresponding Private Key.

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
RootIdentity identity; // a RootIdentity instance
int index = 8; // the specified index
boolean overwrite = true;

DIDDocument doc = identity.newDid(index, overwrite, storePasswd);
```
