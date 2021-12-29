# Customized DID

Customized DID a short DID string is provided by the user, which is also unique. Like a primitive DID, the details of the customized DID are expressed in the DID document.

A Customized DID is generated and managed by at least one primitive DID as a controller. The DID content is recognized and signed by multiple controllers through multi-signature rules. The controller can also take the place of a customized DID to perform certain operations, such as signing.

The Customized DID Document can not contain any form of a key - authorization and entrustment can be completed by the holder’s key.

It also supports adding and deleting holders (it must contain one holder), as well as changing multi-signature rules. When the Customized DID Document changes the holder or the multi-signature rules, it's necessary to complete the document chain operation through the ownership transfer transaction.

Changing the holder is a cautious and rigorous operation. In view of this, the transaction must be completed by using the main key of the DID's original holder based on the modified document and transfer ticket at the same time. Among them, the contents of the transfer ticket are DID topic, the recipient of the transfer ticket, and the transaction ID of the previous DID Document operation. The transfer ticket needs to be signed by the original holder according to the multi-signature rule to show the original holder’s approval of the replacement holder.
