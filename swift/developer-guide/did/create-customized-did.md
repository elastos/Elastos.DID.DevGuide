# Create customized DID

自定义 DID 需要至少有一个有效的持有人，而且持有人的 DID 必须为普通 DID。所以要创建自定义 DID 前，需要准备好持有人的 DID。

To customize DID, at least one valid controller is required, and the controller’s DID must be an ordinary one. Therefore, the controller’s DID should be prepared before the creation of a customized DID.

例如，我们已经拥有一个普通 DID `did:elastos:iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2`，然后创建一个 `did:elastos:ttech` 的自定义 DID，示例如下：

For example, we already have an ordinary DID did:elastos:iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2, and then we create a customized DID of did:elastos:ttech as follows:

```
var store: DIDStore // an opened DIDStore instance
// Create customized DID
let did = try DID("did:elastos:helloworld")
let ttech = try DID("did:elastos:ttech")

// Get the existing DIDDocument
let doc = try store.loadDid(did)

// Create ttech's DID that controller and signed by iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2
let ttechDoc = try doc.newCustomizedDid(withId: ttech, "YOUR-STORE-PASSWORD")

// publish ttech by iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2
ttechDoc.using: "YOUR-STORE-PASSWORD"
```

创建并发布自定义 DID 后，就可以像普通 DID 一样使用，用于身份的表示和验证。 创建多签的自定义 DID 过程相对要复杂，参见创建[多签自定义 DID](create-multi-signed-customized-did.md)。

After a customized DID is created and published, it can be used like an ordinary DID for the representation and verification of identity. It is relatively complicated to create a multi-signed customized DID. See “Create multi-signed customized DID” for details.
