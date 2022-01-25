# Transfer Ownership of the Customized DID

The customized DID is not strongly associated with the authentication key, so the controller of the customized DID can be changed. Modifying the multi-signature rule and the controller of the customized DID are equivalent to a change of the ownership of DID.

To change the ownership of DID, the original controller of DID should sign a TransferTicket and give it to one of the new controllers per the multi-signature rule of the current DID document. The new controller can transfer the ownership of the customized DID by re-creating the DID document, clarifying the new controllers and multi-signature rule, and publishing a new DID to the ID sidechain with the transfer ticket provided by the original controller.

For instance, did:elastos:example is currently owned by Alice, Bob and Carol, which requires 2 signatures of the 3 controllers. The DIDs of Alice, Bob and Carol are as follows:

* Alice - did:elastos:iXkgy7RruwjJDP2o3pfPaxpVR65bEzzjtz
* Bob - did:elastos:iUCXpvpNZ9B9vwWf6PMFMJ6duRPPiLSWSw
* Carol - did:elastos:ignLDwpCumFLA2GrXptr5zNugtMcfvoM4d

The ownership of did:elastos:example should be transferred to Dan and Erin, whose DIDs are as follows:

* Dan - did:elastos:ice6yo4NuazaJP9hwgXxhgmNVGssusEz7p
* Erin - did:elastos:iTsF892QDbDu3JnHXzPbE6udK9b5TZQdTo

## 1. Alice Creates a TransferTicket

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let alice = try DID("did:elastos:iXkgy7RruwjJDP2o3pfPaxpVR65bEzzjtz")
let dan = try DID("did:elastos:ice6yo4NuazaJP9hwgXxhgmNVGssusEz7p")
let example = try DID("did:elastos:example")
// Alice get her document
let aliceDoc = try store.loadDid(alice)
// Create the TransferTicket for example to Dan
let ticket = try aliceDoc.createTransferTicket(withId: example, to: dan, using: storePasswd)
// Serialize the ticket to the string
let serializedTicket = ticket.serialize()
```

Main parameters:

* withId: the customized DID to be transferred
* to: one of the new controllers - Dan or Erin both are okay

The TransferTicket of DID only needs to declare any one of the new controllers. This example declares that the ownership is transferred to Dan.

The TransferTicket created by Alice only contains Alice’s signature, which fails to reach the requirement of 2 signatures. Hence, Alice needs to serialize this document and give it to Bob or Carol to continue signing DIDDocument through the way provided by the application. In this example, this document is assumed to be signed by Bob.

## 2. Bob Signs the TransferTicket

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let bob = try DID("did:elastos:iUCXpvpNZ9B9vwWf6PMFMJ6duRPPiLSWSw")
let serializedTicket: String = ... // the serialized ticket got from Alice
// Bob get his document
let bobDoc = try store.loadDid(bob)
let ticket = try TransferTicket.deserialize(serializedTicket)
// Bob sign the ticket
let signedTicket = trybobDoc.sign(with: ticket, using:storePasswd)
// Serialize the ticket to the string
let finalTicket = ticket.serialize()
```

Once Bob signs the TransferTicket, iy will be valid, for it contains Alice’s and Bob’s signatures, which accords with the multi-signature rules stipulated by example DID.

The current controller of example DID only needs to provide this signed valid transfer ticket to Dan, so that Dan can publish a new DID document with this transfer ticket.

## 3. Dan & Erin Recreate the DID Document

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let dan = try DID("did:elastos:ice6yo4NuazaJP9hwgXxhgmNVGssusEz7p")
let erin = try DID("did:elastos:iTsF892QDbDu3JnHXzPbE6udK9b5TZQdTo")
let example = try DID("did:elastos:example")
// Alice get her document
let danDoc = try store.loadDid(dan)
// Create example's DID
DIDDocument exampleDoc = danDoc.newCustomizedDid(withId: example, [dan, erin], 2, true, storePassword)
// Serialize the document to the string
let serializedDoc = try exampleDoc.serialize()
```

Then submit the document to be signed to Erin:

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let erin = try DID("did:elastos:iTsF892QDbDu3JnHXzPbE6udK9b5TZQdTo")
let serializedDoc: String = ... // the serialized document got from Alice
// Erin get his document
let erinDoc = store.loadDid(erin)
let exampleDoc = try DIDDocument.convertToDIDDocument(fromJson: serializedDoc)
// Erin sign the example's DID document
let signedExampleDoc = erinDoc.sign(with: exampleDoc, using: storePasswd)
```

> Since TransferTicket is for Dan, he must be one of the controllers of the new DID, and his signature must be included in the DID document published for the first time.

## 4. Complete Ownership Transfer by Publishing the New DID Document Using TransferTicket

After Dan and Erin create and sign the valid DID document, any controller can publish a new DID with the obtained transfer ticket of the example to declare the ownership. Suppose Dan publishes the new DID document of example:

```
let store: DIDStore = ...  // an opened DIDStore instance
let storePasswd = "secret"
let dan = try DID("did:elastos:ice6yo4NuazaJP9hwgXxhgmNVGssusEz7p")
let serializedNewExampleDoc: String = ... // the serialized DIDDocument for example
let serializedTicket: String = ... // the serialized TransferTicket
// Get the new DIDDocument for example DID
let exampleDoc = try DIDDocument.convertToDIDDocument(fromJson: serializedNewExampleDoc)
// Parse the TransferTicket
let ticket = TransferTicket.deserialize(serializedTicket)
try exampleDoc.setEffectiveController(dan)
try exampleDoc.publish(with: ticket, using: storePasswd)
```

Parameters:

The document should have an effective controller, otherwise this method will fail.

* did: the target customized DID to be transfer
* to: who the did will transfer to
* storePassword: the password for the DIDStore

> When transferring the ownership of DID, the TransferTicket contains the final chain state of the target DID. Therefore, once the TransferTicket is created, the original controller should not update this DID. If there is an update operation, the previously created TransferTicket will become invalid. If the invalid ticket is used to publish a new document, this transaction will be rejected by the ID sidechain.
