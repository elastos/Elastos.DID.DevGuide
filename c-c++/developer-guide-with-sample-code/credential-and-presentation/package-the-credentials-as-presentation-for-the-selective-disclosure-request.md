# Package the Credentials as Presentation for the Selective Disclosure Request

Verifiable expression refers to a data set containing a subset of verifiable credentials and countersign of an entity, which is used to show its identity to a third party.

Verifiable expression can also be an empty expression entity without verifiable credential.

## Example

```c
//'doc' is holder's document
DID *holder = DIDDocument_GetSubject(doc);
DIDURL *id = DIDURL_NewFromDid(holder, "vp1");
const char *types[2] = {"Trail", "TestPresentation"};

//create Presential by four Credential objects
Presentation *vp = Presentation_Create(id, holder, types, 2, "873172f58701a9ee686f0630204fee59",
                         "https://example.com/", NULL, store, storepass, 4, cred1, cred2, cred3, cred4);
                                       
DIDURL_Destroy(id);
... ... ... ...
Presentation_Destroy(vp);
```

## Usage

```c
Presentation *Presentation_Create(
        DIDURL *id,
        DID *holder,
        const char **types, size_t size,
        const char *nonce,
        const char *realm,
        DIDURL *signkey,
        DIDStore *store,
        const char *storepass,
        int count, ...);
```

```c
Presentation *Presentation_CreateByCredentials(
        DIDURL *id,
        DID *holder,
        const char **types, size_t size,
        const char *nonce,
        const char *realm,
        Credential **creds, size_t count,
        DIDURL *signkey,
        DIDStore *store,
        const char *storepass);
```

Presentation can be generated through the above two methods, with different ways of Credential importing. The former is imported as a variable parameter, while the latter is imported as an array.

id is the ID of Presentation; holder is the holder of Presentation; nonce is a random value used by the signing; realm indicates the applicable domain and address of the presentation; signkey is the Authentication Key used by the holder of the Presentation to sign and encapsulate the Presentation; count is the number of imported Credentials.
