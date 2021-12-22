# Verifiable credential

DID Document原则上是不携带任何个人信息（除内嵌verifiable credential），那么DID拥有者就可以将需要公开或者具体实体的关键信息记录在verifiable credential。

In principle, DID Document does not carry any personal information (except embedded verifiable credential), so the DID owner can record the key information that needs to be disclosed, or which is related to specific entities in verifiable credential.

Elastos DID Document可以内嵌公开的可验证凭证，来支持有DID公开绑定实体信息的需求。

Elastos DID Document can embed public verifiable credentials to support the requirement that DID publicly bind entity information.

Verifiable credential都只能由issuer颁发，该issuer就是一个DID。当然这个DID拥有者可以是个人也可以是某可信机构。

Verifiable credential can only be issued by an issuer, which is a DID. Of course, the DID owner can be an individual or a trusted organization.

Verifiable credential 只有两种公开方式，一种是内嵌在DID Document；另一种是封装在verifiable presentation里。&#x20;

There are only two ways to expose verifiable credential, one is embedded in DID Document; The other is encapsulated in a verifiable presentation.
