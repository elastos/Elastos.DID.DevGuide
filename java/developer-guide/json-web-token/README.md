# Json Web Token

At present, JSON Web Token (JWT) is widely used, and the key of DID itself can be used to sign and verify the token for JWT. If external JWT implementation is adopted, it's necessary to export the key from DID and provide it to the third parties' JWT library. This process requires the intervention of developers, and there are many security risks associated with this.

Therefore, Elastos DID has built-in JWT support. JWT can be directly created and verified using the interfaces and methods provided by DID SDK.
