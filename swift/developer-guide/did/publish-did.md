# Publish DID

When a DID serves as an identity identifier, it's usually necessary to publish this DID first so that the verifier can analyze and verify it conveniently. The publication of DIDs includes initial creation and updating:

* Create: to publish the newly created DID to the ID sidechain for the first time
* Update: to update the previously published DID

The DID SDK will automatically construct the corresponding ID transaction according to the state of DID in the chain. The constructed ID transaction needs to be signed with the valid key of DID, which means that only the controller of this DID can publish it to the chain. Here's an example of creating and publishing a new DID:

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let identity: RootIdentity = ... // a RootIdentity instance
// Get the new created DIDDocument
let doc = try identity.newDid(storePasswd)
// Publish the DID to the ID side chain using the default key
try doc.publish(using: storePasswd)
```

By default, the published DID is signed by the default authentication key corresponding to this DID. In the above example, the transaction is signed by the default key - this is also the mode used by most applications. If a DID sets multiple authentication keys and specifies one to sign the transaction, then the ID of the target key can be specified in the publishing method, and the PrivateKey corresponding to this key is required to be in the DIDStore where DIDDocument is located. For example:

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"

let doc: DIDDocument = ... // the DIDDocument to be publish
// The key id should defined in the document, also the private key in the store
let signKey = try DIDURL("did:elastos:iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2#secondary")

// Publish the DID to the ID side chain using the default key
try doc.publish(with: signKey, using: storePasswd)
```

The publish operation when updating DID is the same as that when creating DID. The DID SDK will automatically process the transaction type according to the DID status on the chain. Below is an example of updating a published DID document object:

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let did = try DID("did:elastos:iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2")
// Get the existing DIDDocument
let doc = try store.loadDid(did)
let db = try doc.editing()
// Do some modification on the document
let newDoc = db.seal(using: storePasswd)
// Update the existing DIDDocument on the ID side chain
try doc.publish(using:storePasswd)
```

The publication of customized DIDs is similar to that of ordinary DIDs, and the published transaction can be signed by the valid authentication key or declared authentication key of any controller. For example:

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
// ttech controlled by iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2
// and the store has the default private key for iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2
let did = try DID("did:elastos:iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2")
let ttech = try DID("did:elastos:ttech")
// Get the existing DIDDocument
let ttechDoc = try store.loadDid(ttech)
// iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2 on beharf of ttech
try ttechDoc.setEffectiveController(did)
// publish ttech by iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2
ttechDoc.publish(using: storePasswd)
```

The publish operation in the above example is similar to the publishing of ordinary DID. Alternatively, you can specify the valid authentication key of the current controller to sign the transaction.

Transfer the document and publish it to the chain

* Create TransferTicket in the first place
* Parameter did: for DID to be transferred
* to: did that accepts the transfer (to whom)
* storePassword: local private key/encrypted password

```
// create new controller
let newController = try identity.newDid(storePassword)
let ticket = try controller.createTransferTicket(withId: did, to: newController.subject, using: storePassword)
// create new document for customized DID
let newDoccument = try newController.newCustomizedDid(withId: did, true, storePassword)
```

* > Every DID document has a validity period. In principle, the DID will be invalid after it expires, so it 's necessary to update the DID document, prolong the validity period, and publish it before the expiration. Besides, the deactivated DID is permanently invalid, which cannot be updated or published.
