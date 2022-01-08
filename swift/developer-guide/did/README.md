# DID

DIDs represents the identities of users - a user can have many DIDs to be applied in different scenarios. DID is just an identifier of a character string, with each one corresponding to a DIDDocument. DID and DIDDocument form a pair of existents that are similar to two sides of a coin.

&#x20;Elastos DID supports two types of DIDs, namely the ordinary and customized ones. The identifier of ordinary DIDs is a base58 character string derived from DID’s default public key through Hash operation, which cannot be directly specified by users. The identifier of the customized DID is specified by the users.

In a normal case, the DID owner needs to publish DID to the ID sidechain before using it. Given a DID identifier, the corresponding DIDDocument can be parsed through the ID sidechain. Therefore, in daily use, the identifier of DID expresses the user’s identity, and the application can obtain DIDDocument through the identifier and conduct the necessary operations related to authentication.

## Ordinary DID

Due to the correlation between the identifier and default key pair of an ordinary DID, a trusted root for verifying this DID is formed, and a complete DID verification chain is generated on this basis, which ensures the security and credibility of the DID identity.

Because of the correspondence between the identifier and key of an ordinary DID, ownership transfer cannot be carried out in principle, because the validity of the transfer cannot be guaranteed.

## Customized DID

The identifier of the customized DID can be specified by users themselves, and there is no trusted root similar to that of the ordinary DID. Therefore, instead of existing independently, the customized DID must be created, owned, and controlled by ordinary DID, and the trusted root should be indirectly established to form a trusted validation chain. There's no difference between customized and ordinary DIDs, because the customized ones can also be used independently.

The customized DID supports transfer ownership, that is, the change of the controller. See [Transfer the ownership of the customized DID](transfer-the-ownership-of-the-customized-did.md) for more details.

The customized DID can be owned by one or more controllers. In the case of a single controller, this controller possesses all the ownership of the customized DID, and any modification and signature of the document of this DID can only be completed by this controller. If the customized DID is held by several controllers, a multi-signature rule can be defined, that is, specifying that the signatures of several controllers are required for the customized DID document to take effect. For instance, if a customized DID is held by three controllers and the signatures of any two controllers can serve as those of the DID document, the multi-signature rule that requires 2 out of 3 signatures can be specified when the document is created. This multi-signature rule applies to any modification of the document.
