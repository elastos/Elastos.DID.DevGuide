# Create DIDs from the RootIdentity

在 Elastos DID SDK 中，所有的普通 DID 都是从 RootIdentity 创建，每个创建的 DID 对应一个整数索引值，索引的值空间是 0 ~ 2,147,483,647。每个索引值和其创建的 DID 是唯一的对应关系，即该索引创建的 DID 永远是一致的。

从 RootIdentity 创建 DID 时，新创建的 DID 对应的 DIDDocument 和 PrivateKey 默认会保存到 RootIdentity 所在的 DIDStore 中。新创建的 DID 不会自动进行发布，仅仅是一个本地的 DID 对象，如果需要发布，那么需要显式的进行发布操作，参见[发布 DID](../did/publish-did.md)。

## 使用默认索引创建 DID

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
RootIdentity identity; // a RootIdentity instance

DIDDocument doc = identity.newDid(storePasswd);
```

使用默认索引创建 DID 时，不需要指定索引值，RootIdentity 会使用当前记录的下一个可用索引创建，并且自动更新下一个可用索引。**推荐使用这种方式创建新的 DID**。

## 指定索引创建 DID

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
RootIdentity identity; // a RootIdentity instance
int index = 8; // the specified index

DIDDocument doc = identity.newDid(index, storePasswd);
```

在指定索引创建 DID 时，指定的索引值不被 RootIdentity 管理，所以需要开发者或者应用来管理可用的索引，并避免重复使用索引引起的冲突。

## 重新创建 DID 对象和其文档

如果需要重新创建已经创建过的 DIDs，默认情况下会因为 DIDs 已经存在而失败，可以使用具有 overwrite 参数的方法创建，如果 overwrite 为 true 的时候，DID SDK 将忽略已经存在的 DID 对象，并重新创建对应的 DID 文档对象，并重新生成其对应的 PrivateKey。

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
RootIdentity identity; // a RootIdentity instance
int index = 8; // the specified index
boolean overwrite = true;

DIDDocument doc = identity.newDid(index, overwrite, storePasswd);
```



