# Create Multi-signed Customized DID

Before creating a multi-signed customized DID, it's necessary for multiple controllers of the DID to reach a consensus, determine the multi-signature rule, and complete the creation process of the DID according to the rule.

The expression of the customized DID multi-signature rule is m:n, that is, m of n. It means that DID is held by n controllers, and any change of DID requires the signatures of m controllers to be valid.

The creation of multi-signature DID needs the DID of one of the n controllers to initiate the creation of the customized DID, and complete the follow-up signature supplement according to the multi-signature rule defined by DID Document until m valid signatures are reached. This is so DID Document can become a valid document, and it can be published on the chain.

The example here is multi-signature DID of 2 of 3. Assume that there are already three controllers, and DIDs are as follows:

* Alice - did:elastos:iXkgy7RruwjJDP2o3pfPaxpVR65bEzzjtz
* Bob - did:elastos:iUCXpvpNZ9B9vwWf6PMFMJ6duRPPiLSWSw
* Carol - did:elastos:ignLDwpCumFLA2GrXptr5zNugtMcfvoM4d

They are going to create 2:3 multi-signature DID did:elastos:example.

## 1. Alice Created the Initial Document First

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID alice = new DID("did:elastos:iXkgy7RruwjJDP2o3pfPaxpVR65bEzzjtz");
DID bob = new DID("did:elastos:iUCXpvpNZ9B9vwWf6PMFMJ6duRPPiLSWSw");
DID carol = new DID("did:elastos:ignLDwpCumFLA2GrXptr5zNugtMcfvoM4d");
DID example = new DID("did:elastos:example");

// Alice get her document
DIDDocument aliceDoc = store.loadDid(alice);

// Create example's DID
DIDDocument exampleDoc = aliceDoc.newCustomizedDid(
    example,                         // the new customized DID
    new DID[] { alice, bob, carol }, // the 3 controllers
    2,                               // needs 2 signatures
    storePasswd);

// Serialize the document to the string
String serializedDoc = exampleDoc.serialize();
```

Alice created the customized DID example - the controllers were Alice, Bob, and Carol, and it was specified that two signatures were needed, that is, 2 of 3. However, this DID Document instance was not a valid document, as it only contained Alice’s signature at present and didn’t reach the requirement of two signatures. Alice needed to serialize this document and give it to Bob or Carol to continue signing DID Document through the way provided by the application. This example assumes that Bob will sign it.

## 2. Bob Signed the Document

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID bob = new DID("did:elastos:iUCXpvpNZ9B9vwWf6PMFMJ6duRPPiLSWSw");
String serializedDoc; // the serialized document got from Alice

// Bob get his document
DIDDocument bobDoc = store.loadDid(bob);
DIDDocument exampleDoc = DIDDocument.parse(serializedDoc);

// Bob sign the example's DID document
DIDDocument signedExampleDoc = bobDoc.sign(exampleDoc, storePasswd);

// Bob can publish the example's DID
signedExampleDoc.setEffectiveController(bob);
signedExampleDoc.publish(storePasswd);
```

After Bob signed DID Document, the DID Document instance obtained contained two signatures from Alice and Bob, which satisfied the 2:3 multi-signature rule. This document was a valid DID Document object and could be used to publish DID.

The publication of customized DID could be published by any controller with its own valid authentication public key, while the above example was published directly by Bob after signing the document.

## 3. Usage of Customized DID

After publishing the DID of example, Alice, Bob, and Carol could all get the DID document from the chain, as well as use the identity of example with their DID as a valid controller. However, they could only use the DID identity without changing the DID document used in the example. If it needs to be changed, it's also necessary to complete multi-signature according to the above process.

The change of DID document should not involve the change of controller or multi-signature rule. If it's necessary to modify these two items, it belongs to the change of DID ownership, and the change process refers to the [ownership change of customized DID](transfer-the-ownership-of-the-customized-did.md).
