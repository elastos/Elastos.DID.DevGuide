# DID

A DID object is representative of the identity of each user. Every user can have many DIDs, which can be used in different scenarios. DID is just an identifier of a string, and each has a corresponding DID Document - both of these go hand and hand together. Two kinds of DIDs are supported by Elastos DID. One is primitive DID, and its identifier is a base58 string derived from DID’s default public key through Hash operation, which can't be directly specified by users. The other is customized DID, that is, the identifier of DID is specified by the user.

Usually, before the usage of DID, it's necessary to publish DID to the ID sidechain. The corresponding DID Document can be parsed through the ID sidechain, when the DID identifier is given. Therefore, in daily use, as long as the identifier of DID is provided, the user’s identity can be expressed, and the application can obtain DID Document through the identifier as well as carry out necessary operations related to authentication.

## Primitive DID

Because of the relationship between the DID identifier of the primitive DID and the default key, a trusted root for verifying the DID is formed, and a complete DID verification chain is formed according to this, which ensures the security and credibility of the DID identity.

Owing to the correspondence between primitive DID identifier and key, in principle, ownership transfer cannot be carried out, for the validity of the transfer cannot be guaranteed.

## Customized DID

The identifier of the customized DID can be specified by the user, and there is no trusted root similar to the primitive DID, thus the customized DID cannot exist independently but must be created, owned, and controlled by the primitive DID - the trusted root can be indirectly established to form a trusted verification chain for the DID. Customized DID can be used independently, which is no different from primitive one.

Customized DID can transfer ownership, that is, change the controller. See the [changing of ownership of customized DID ](transfer-the-ownership-of-the-customized-did.md)for details.

Customized DID can be held by one or more controllers. In the case of a single controller, this controller owns all the ownership of customized DID, and any modification and signature of the document of the customized DID can only be completed by this controller. In the case of multiple controllers, a rule of multiple signatures can be defined, that is, you can specify that the signatures of several controllers are required for the customized DID document to take effect. For example, if a customized DID is held by three controllers, and the signatures of any two controllers can be used as the signatures of the DID document, the multi-signature rule of 3 of 2 can be specified when the document is created. This multi-signature rule applies to any modification of the document.
