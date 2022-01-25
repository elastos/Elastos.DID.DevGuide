# Create Customized DID

To customize DID, at least one valid controller is required, and the controller’s DID must be an ordinary one. Therefore, the controller’s DID should be prepared before the creation of a customized DID.

For example, we already have an ordinary DID: did:elastos:iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2

Then we create a customized DID of did:elastos:ttech as follows:

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

After a customized DID is created and published, it can be used like an ordinary DID for the representation and verification of identity. It's relatively complicated to create a multi-signed customized DID - see [Create multi-signed customized DID](create-multi-signed-customized-did.md) for details.
