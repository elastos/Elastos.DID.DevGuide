# Json Web Token

JSON Web Token (JWT) is currently extensively used, and the key of the DID itself can be used to sign and verify the token for JWT. If external JWT implementation is applied, it's inevitable to export the key pair of DID from the DID and provide it to the JWT library of a third party. This process requires the intervention of developers, so there are many security risks.

Hence, Elastos DID is equipped with built-in JWT support. JWT can be created and verified directly by using the interfaces and methods provided by the DID SDK.
