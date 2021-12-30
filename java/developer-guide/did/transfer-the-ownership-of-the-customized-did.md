# Transfer Ownership of the Customized DID

Customized DID can change the controller because it's not strongly associated with the authentication key. For the customized DID, modifying the multi-signature rule and changing the controller are all changes to the ownership of DID.

Changing the ownership of DID requires the controller of the original DID to sign a Transfer Ticket to one of the new controllers according to the multi-signature rule of the current DID document. The new controller can re-create the DID document, declare new controllers/multi-signature rules, and issue a new DID-to-ID sidechain using the Transfer Ticket provided by the original controller, thus realizing the transfer of DID ownership.

For example, presently, the 2 of 3 DIDs did:elastos:example are held by Alice, Bob and Carol. Their DIDs are:

* Alice - did:elastos:iXkgy7RruwjJDP2o3pfPaxpVR65bEzzjtz
* Bob - did:elastos:iUCXpvpNZ9B9vwWf6PMFMJ6duRPPiLSWSw
* Carol - did:elastos:ignLDwpCumFLA2GrXptr5zNugtMcfvoM4d

Now, the ownership of did:elastos:example needs to be transferred to Dan and Erin, whose DIDs are:

* Dan - did:elastos:ice6yo4NuazaJP9hwgXxhgmNVGssusEz7p
* Erin - did:elastos:iTsF892QDbDu3JnHXzPbE6udK9b5TZQdTo

## 1. Alice Created a Transfer Ticket

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID alice = new DID("did:elastos:iXkgy7RruwjJDP2o3pfPaxpVR65bEzzjtz");
DID dan = new DID("did:elastos:ice6yo4NuazaJP9hwgXxhgmNVGssusEz7p");
DID example = new DID("did:elastos:example");

// Alice get her document
DIDDocument aliceDoc = store.loadDid(alice);

// Create the TransferTicket for example to Dan
TransferTicket ticket = aliceDoc.createTransferTicket(
    example,   // the customized DID to be transfer
    dan,       // one of the new controllers, Dan or Erin both are ok
    storePasswd);

// Serialize the ticket to the string
String serializedTicket = ticket.serialize();
```

In DID’s ownership Transfer Ticket, we needed to declare one of the new controllers. The DID example was declared to be transferred to Dan.

The Transfer Ticket created by Alice only received her signature, which didn’t reach the minimum requirement of two. So, Alice needed to serialize this document and give it to Bob or Carol to continue signing through the way provided by the application. This example assumes that Bob would sign it.

## 2. Bob Signed the Transfer Ticket

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID bob = new DID("did:elastos:iUCXpvpNZ9B9vwWf6PMFMJ6duRPPiLSWSw");
String serializedTicket; // the serialized ticket got from Alice

// Bob get his document
DIDDocument bobDoc = store.loadDid(bob);
TransferTicket ticket = TransferTicket.parse(serializedTicket);

// Bob sign the ticket
TransferTicket signedTicket = bobDoc.sign(ticket, storePasswd);

// Serialize the ticket to the string
String finalTicket = ticket.serialize();
```

After Bob signed the Transfer Ticket, it already contained signatures of Alice and Bob, which met the requirement of the multi-signature rule.

The current controller of example DID only needed to hand over this signed and valid Transfer Ticket to Dan, and Dan could use this ticket to publish a new DID document.

## 3. Dan and Erin Recreated the DID Document

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID dan = new DID("did:elastos:ice6yo4NuazaJP9hwgXxhgmNVGssusEz7p");
DID erin = new DID("did:elastos:iTsF892QDbDu3JnHXzPbE6udK9b5TZQdTo");
DID example = new DID("did:elastos:example");

// Alice get her document
DIDDocument danDoc = store.loadDid(dan);

// Create example's DID
DIDDocument exampleDoc = danDoc.newCustomizedDid(
    example,                         // the new customized DID
    new DID[] { dan, erin },         // the 3 controllers
    2,                               // needs 2 signatures
    storePasswd);

// Serialize the document to the string
String serializedDoc = exampleDoc.serialize();
```

Then Erin would sign the document:

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID erin = new DID("did:elastos:iTsF892QDbDu3JnHXzPbE6udK9b5TZQdTo");
String serializedDoc; // the serialized document got from Alice

// Erin get his document
DIDDocument erinDoc = store.loadDid(erin);
DIDDocument exampleDoc = DIDDocument.parse(serializedDoc);

// Erin sign the example's DID document
DIDDocument signedExampleDoc = erinDoc.sign(exampleDoc, storePasswd);
```

> Because this Transfer Ticket is for Dan, Dan must be one of the controllers of the new DID, and the first published DID document must contain his signature.

## 4. Use Transfer Ticket to Publish a New DID Document to Complete the Ownership Transfer

After Dan and Erin created and signed a valid DID document, anyone could issue a new DID with the example ticket previously obtained to declare the ownership of it. Suppose Dan published the new DID document example:

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID dan = new DID("did:elastos:ice6yo4NuazaJP9hwgXxhgmNVGssusEz7p");
String serializedNewExampleDoc; // the serialized DIDDocument for example
String serializedTicket; // the serialized TransferTicket

// Get the new DIDDocument for example DID
DIDDocument exampleDoc = DIDDocument.parse(serializedNewExampleDoc);
// Parse the TransferTicket
TransferTicket ticket = TransferTicket.parse(serializedTicket);

exampleDoc.setEffectiveController(dan);
exampleDoc.publish(ticket, storePasswd);
```

> When transferring ownership of DID, the Transfer Ticket contains the last chain state of the target DID - once the Transfer Ticket is created, the original controllers shouldn't update it. If there is an update operation, the previously created Transfer Ticket will become invalid. When a new document is published using the invalid ticket, the ID sidechain will reject this transaction.
