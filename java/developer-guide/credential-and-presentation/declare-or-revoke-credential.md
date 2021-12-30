# Declare or Revoke Credential

## Disclose Credential

Generally speaking, a credential contains personal information, which should be held safely by individuals if personal privacy is desired. However, the DID entity needs to disclose its credentials in some scenarios or applications, so Elastos DID provides support for issuing credentials on the ID sidechain.

It should be noted that once the credential is published, it means that any personal information contained in the credential will be permanently disclosed and cannot be deleted or withdrawn.

Credential issuing requires the controller to use his own DID authentication to issue it. For example:

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DIDURL vcId = new DIDURL("did:elastos:ietN9jBeA4NpSKeFMoT9GcTjpUegWHmFr2#gamescore");

VerifiableCredential vc = store.loadCredential(vcId);
vc.declare(storePasswd);
```

Because DID will be used to sign the publishing transaction, the DID document and private key of the controller are needed in the DID store.

## Credential Revocation

A credential, whether it's made public or not, can be revoked. The revoked one can no longer be used, and the validity verification will fail. The credential can be revoked by its issuer or controller.

Revocation means that the credential identification is made public on the chain, but its contents are not made public. If the credential has been made public, its publicity will not be changed.

### Controller Revokes the Credential

```
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DIDURL vcId = new DIDURL("did:elastos:ietN9jBeA4NpSKeFMoT9GcTjpUegWHmFr2#gamescore");

VerifiableCredential vc = store.loadCredential(vcId);
vc.revoke(storePasswd);
```

Because DID will be used to sign the publishing transaction, the DID document and private key of the controller are needed in the DID store.

## Issuer Revokes the Credential

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID did = new DID("did:elastos:ietN9jBeA4NpSKeFMoT9GcTjpUegWHmFr2");
// credential instance issued by ietN9jBeA4NpSKeFMoT9GcTjpUegWHmFr2 that to be revoke 
VerifiableCrdential vc;

DIDDocument issuer = store.loadDid(doc);
credential.revoke(issuer.getDefaultPublicKeyId(), TestConfig.storePass);
```
