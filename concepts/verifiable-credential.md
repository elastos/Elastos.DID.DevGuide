# Verifiable Credential

In principle, DID Document does not carry any personal information (except embedded verifiable credentials), so the DID owner can record the key information that needs to be disclosed, or that which is related to specific entities in verifiable credential.

The Elastos DID Document can embed public verifiable credentials to support the requirement that DID publicly binds entity information. Verifiable credential can only be provided by an issuer, which is essentially a DID. Of course, the DID owner can be an individual or a trusted organization.

There are only two ways to expose verifiable credential - one is embedding it in DID Document, and the other is encapsulating it in a verifiable presentation.
