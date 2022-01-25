# Metadata: DID Objects

To better develop the applications, DIDStore provides its stored DID objects with the ability to save elements of local data - the DID SDK refers to such data as the metadata of DID objects.

The implementations of different DID objects provide different metadata according to their own needs. DIDMetadata and CredentialMetadata are two typical examples. Both DID and verifiable credentials can get their metadata interfaces from the DID objects and access related data. For example:

```
let store: DIDStore = ... // an opened DIDStore instance
let did = try DID("did:elastos:iU7yMyvoqEqJQgjX71W627whs1MGmEM5j3")
let vcId = try DIDURL("did:elastos:iU7yMyvoqEqJQgjX71W627whs1MGmEM5j3#profile")

let doc = try store.load(did)
let dm = doc.getMetadata()
dm.setAlias("Primary DID")
// ...

let vc = store.loadCredential(byId: vcId)
let cm = vc.metadata
String alias = cm.getAlias()
// ...
```

The interface of Metadata may differ in SDKs implemented in different languages, so it should be used reasonably according to specific SDKs.

Moreover, RootIdentity and DIDStore also use the metadata mechanism provided by the store. Due to the need for implementation, the metadata interface is not directly provided, but the metadata itself will also be used by the local data of their objects.
