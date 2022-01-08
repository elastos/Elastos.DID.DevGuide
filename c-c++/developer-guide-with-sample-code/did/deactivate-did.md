# Deactivate DID

The Publish and Transfer DID methods are both effective cochain, and deactivated DID is the invalid cochain, so the DID is deactivated.

The DID can be deactivated by itself or the principal. Primitive DID can be deactivated by Authentication Key and Authorization Key. Customized DID can be deactivated by Authentication Key and Controller’s Default Key.

The deactivation cannot be performed by a DID that isn't on the chain.

## Usage

```c
int DIDDocument_DeactivateDID(
        DIDDocument *document,
        DIDURL *signkey,
        const char *storepass);
```

The method is initiated by the deactivated DID itself, and the deactivating is completed with its own Authentication Key.

signKey s the Authentication Key of the designated signature deactivation transaction. If signKey is null, the Default Key will be used by default.

Other parameters are the same as the Publish DID method.

```c
int DIDDocument_DeactivateDIDByAuthorizor(
        DIDDocument *document,
        DID *target,
        DIDURL *signkey,
        const char *storepass);
```

This method is a deactivating operation initiated by the principal.

target is the DID waiting to be deactivated.

signKey: the deactivation of primitive DID is initiated by Autherizor DID Document, which provides Autherizor Authentication Key and must also exist as Authorization Key in DID Document of Target. Customized DID is initiated by Controller DID Document, and Controller Default Key is provided as Sign Key.

Other parameters are the same as Publish DID.
