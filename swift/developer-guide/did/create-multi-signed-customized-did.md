# Create Multi-signed Customized DID

Before creating a multi-signed customized DID, multiple controllers of the DID should reach a consensus, determine the multi-signature rule, and complete the creation process of the DID according to this scheme.

The multi-signature rule of the customized DID is m:n - that is, m out of n should be signed. It means that the DID is held by n controllers, and any change of DID requires the signatures of m controllers to take effect.

To create a multi-signed DID, the DID of one of the n controllers is needed to initiate the creation of the customized DID, and supplement the subsequent signatures according to the multi-signature rule defined by DIDDocument until there are m valid signatures. This enables the DIDDocument to become a valid document to be published on the chain.

The example here is a multi-signed DID that requires 2 signatures out of 3 signatures. Assume that there are 3 controllers whose DIDs are as follows:

* Alice - did:elastos:iXkgy7RruwjJDP2o3pfPaxpVR65bEzzjtz
* Bob - did:elastos:iUCXpvpNZ9B9vwWf6PMFMJ6duRPPiLSWSw
* Carol - did:elastos:ignLDwpCumFLA2GrXptr5zNugtMcfvoM4d

They need to create a 2:3 multi-signed DID did:elastos:example.

## 1. Alice Creates  the Initial Document

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let alice = try DID("did:elastos:iXkgy7RruwjJDP2o3pfPaxpVR65bEzzjtz")
let bob = try DID("did:elastos:iUCXpvpNZ9B9vwWf6PMFMJ6duRPPiLSWSw")
let carol = try DID("did:elastos:ignLDwpCumFLA2GrXptr5zNugtMcfvoM4d")
let example = try DID("did:elastos:example")
// Alice get her document
let aliceDoc = try store.loadDid(alice)
// Create example's DID
let exampleDoc = try aliceDoc.newCustomizedDid(withId: example, [alice, bob, carol], 2, storePassword)
// Serialize the document to the string
let serializedDoc = exampleDoc.toString()
```

Alice creates the customized DID, with Alice, Bob and Carol being the controllers. It's specified that two signatures are required, so 2 out of the 3 controllers need to sign. However, in this example, the DIDDocument is not valid because it only contains Alice’s signature, while it's supposed to contain 2. Hence, Alice needs to serialize this document and give it to Bob or Carol to continue signing DIDDocument through the way provided by the application. In this example, this document is assumed to be signed by Bob.

## 2. Bob Signs the Document

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let bob = try DID("did:elastos:iUCXpvpNZ9B9vwWf6PMFMJ6duRPPiLSWSw")
let serializedDoc // the serialized document got from Alice
// Bob get his document
let bobDoc = try store.loadDid(bob)
let exampleDoc = try DIDDocument.convertToDIDDocument(fromJson: serializedDoc)
//loaded Bob sign the example's DID document
let signedExampleDoc = bobDoc.sign(with: exampleDoc, using: storePasswd)
// Bob can publish the example's DID
try signedExampleDoc.setEffectiveController(bob)
try signedExampleDoc.publish(storePasswd)
```

## 3. Use the Customized DID

After publishing the DID in this example, Alice, Bob and Carol can get this DID document directly from the chain and use the identity of the example with their own DIDs as the effective controllers. Nevertheless, they can only use the DID identity rather than change the DID document in the example. If any change is required, the document should be multi-signed according to the above process.

The change of DID document should not involve the multi-signature rule or the change of the controller. If these two items are revised, the DID ownership will be changed. Please refer to //TODO: for the change process.

Parameters:

* DID: the new customized identifier
* Controllers: the other controllers for the new customized DID
* Multisig: how many signatures are required
* storePassword: the password for the DIDStore
