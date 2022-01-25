# Declare or Revoke Credential

## Declare Credentials

Generally, since credentials contain personal information, they should be held safely by individuals from the perspective of personal privacy. However, DID entities need to declare their own credentials in some scenarios or applications. Elastos DID supports the issuance of credentials on the ID sidechain.

It should be noted that once the credential is published, the personal information contained in the credential will be permanently disclosed and cannot be deleted or withdrawn.

Only through authentication with his/her own DID can the credential holder publish the credential. For example:

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let vcId = try DIDURL("did:elastos:ietN9jBeA4NpSKeFMoT9GcTjpUegWHmFr2#gamescore")
let vc = try store.loadCredential(byId: vcId)
try vc.declare(storePasswd)
```

The DID is needed for signing the published transaction. Therefore, the DID store should include the DID document of the credential holder and private key needed for signing.

## Revoke Credentials

A credential, whether declared or not, can be revoked. The revoked credential can no longer be used, and the validity verification will fail as well. The credential can be revoked by the issuer or holder of the credential.

To revoke the credential is to declare the credential identifier on the chain, but the content of the credential will not be made public. If the credential has been made public, its publicity will not be changed.

### The Holder Revokes the Credential

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let vcId = try DIDURL("did:elastos:ietN9jBeA4NpSKeFMoT9GcTjpUegWHmFr2#gamescore")
let vc = try store.loadCredential(byId: vcId)
try vc.revoke(storePasswd)
```

The DID is needed for signing the published transaction. Therefore, the DID store should include the DID document of the credential holder and the private key needed for signing.

### The Issuer Revokes the Credential

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let did = try DID("did:elastos:ietN9jBeA4NpSKeFMoT9GcTjpUegWHmFr2")
// credential instance issued by ietN9jBeA4NpSKeFMoT9GcTjpUegWHmFr2 that to be revoke 
let vc: VerifiableCrdential = ...
let issuer = try store.loadDid(doc)
credential.revoke(issuer.defaultPublicKeyId(), storePasswd)
```
