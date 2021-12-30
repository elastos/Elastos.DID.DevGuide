# Create Customized DID

Customized DID requires at least one valid controller, and the controller’s DID must be primitive DID. Therefore, before creating a customized DID, you need to prepare the controller's DID.

For example, we already have a primitive DID did:elastos:iceqy638grfnx6vgp3utvyr5du79xrl6, and then create a customized DID of did:elastos:foobar. The example is as follows:

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

After creating and publishing the customized DID, it can be used like primitive DID for identity representation and verification.

The process of creating multi-signed customized DID is relatively complicated. See [creation of multi-signed customized DID](create-multi-signed-customized-did.md).
