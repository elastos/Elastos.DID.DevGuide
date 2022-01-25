# Resolve the Declared Credentials

The public credential is published on ID sidechain and can be publicly retrieved. The DID SDK provides a parsing method of credential, which can parse the corresponding credential object from the credential identifier. For example:

```java
DIDURL vcId = new DIDURL("did:elastos:imXPgH1Bx2uuUFaemJ9vwf6dVbqLWDvMiT#gamescore");

VerifiableCredential vc = VerifiableCredential.resolve(vcId);
```

The DID Backend needs to be initialized before the parsing of the credential. Most of the DID operations need to initialize the DID Backend as well. See the [DID Backend initialization](../didbackend/) for details.

The parsing implementation of Java SDK has a [local cache mechanism](../didbackend/#didbackend-cache), similar to the browser’s cache, which is used to improve the efficiency of parsing. When the local cache is valid, resolve will load the credential object from the local cache first. The application can set these parameters according to requirements when initializing DID Backend.

If the application needs to ignore the results of the local cache and force the re-parsing of specific credentials from the ID sidechain, it can be specified by parameters. For example:

```java
DIDURL vcId = new DIDURL("did:elastos:imXPgH1Bx2uuUFaemJ9vwf6dVbqLWDvMiT#gamescore");
boolean force = true;

VerifiableCredential vc = VerifiableCredential.resolve(vcId, force);
```
