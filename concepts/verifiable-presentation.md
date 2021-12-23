# Verifiable Presentation

Verifiable presentation refers to a data set containing a subset of verifiable certificates and countersigns of an entity, which is used to show its identity to a third party. Generally speaking, the verifiable presentation contains vouchers for one DID identity, which can be issued by different third-party entities to express its identity information.

The verifiable presentation can contain multiple vouchers or none at all, which are formed once and cannot be modified. Moreover, it can only encapsulate its own verifiable credential, and its holder signs the result. Therefore, the holder of the verifiable presentation is the subject of the verifiable credential, otherwise, the verifiable presentation is invalid.
