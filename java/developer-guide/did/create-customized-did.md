# Create customized DID

自定义 DID 需要至少有一个有效的持有人，而且持有人的 DID 必须为普通 DID。所以要创建自定义 DID 前，需要准备好持有人的 DID。

例如，我们已经拥有一个普通 DID `did:elastos:icEQY638GRFRNx6vgP3UTVYr5Dwu79xRL6`，然后创建一个 `did:elastos:foobar` 的自定义 DID，示例如下：

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID did = new DID("did:elastos:icEQY638GRFRNx6vgP3UTVYr5Dwu79xRL6");
DID foobar = new DID("did:elastos:foobar");

// Get the existing DIDDocument
DIDDocument doc = store.loadDid(did);

// Create foobar's DID that controller and signed by icEQY638GRFRNx6vgP3UTVYr5Dwu79xRL6
DIDDocument foobarDoc = doc.newCustomizedDid(foobar, TestConfig.storePass);

// publish foobar by icEQY638GRFRNx6vgP3UTVYr5Dwu79xRL6
foobarDoc.publish(storePasswd);
```

创建并发布自定义 DID 后，就可以像普通 DID 一样使用，用于身份的表示和验证。

创建多签的自定义 DID 过程相对要复杂，参见创建[多签自定义 DID](create-multi-signed-customized-did.md)。
