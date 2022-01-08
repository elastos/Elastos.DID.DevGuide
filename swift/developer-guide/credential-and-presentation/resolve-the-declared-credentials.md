# Resolve the Declared Credentials

All declared credentials are published on the ID sidechain where they can be publicly retrieved. The DID SDK offers a method of resolving the declared credentials, which can resolve the corresponding credential objects from the credential identifiers. For example:

```
let vcId = try DIDURL("did:elastos:imXPgH1Bx2uuUFaemJ9vwf6dVbqLWDvMiT#gamescore")
let vc = try VerifiableCredential.resolve(vcId)
```

DIDBackend should be initialized before resolving the credentials - it should be initialized in most of the other DID operations as a general rule of thumb. See [Initialize DIDBackend](../didbackend/) for details.

The resolve implementation of Swift SDK has a local cache mechanism similar to the browser’s cache, which is used to improve the resolving efficiency. When the local cache is valid, resolve will load the credential object from the local cache first. Applications can set these parameters according to [requirements when initializing DIDBackend](../didbackend/#didbackend-cache).

If applications need to ignore the results of local cache and force the re-resolution of specific credentials from the ID sidechain, it can be specified by certain parameters. For example:

```
let vcId = try DIDURL("did:elastos:imXPgH1Bx2uuUFaemJ9vwf6dVbqLWDvMiT#gamescore")
let force = true
let vc = try VerifiableCredential.resolve(vcId, force)
```
