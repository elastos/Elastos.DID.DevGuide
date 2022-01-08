# Create DIDDocument

DID Document is divided into primitive DID Document and customized DID Document, and the contents here are all aimed at the primitive DID Document. Customized DID Document will be described in the following sections.

Generate DID Document by newDid, which is the most basic DID Document containing only the default key. Users can use DID Document Builder to modify DID Document content, complete the modification, and terminate the modification by seal method to get a new DID Document.

DID Document has five elements: public key, authentication key, authorization key, verifiable credential, and service. You can modify the document by adding or removing them accordingly. Refer to API doc for specific methods.

## Example

```c
//get did document builder
DID *did = DIDDocument_GetSubject(doc);
DIDDocumentBuilder *builder = DIDDocument_Edit(doc, NULL);

// Add 2 public keys
DIDURL *id1 = DIDURL_NewFromDid(did, "test1");
... ... ... ... 
if (DIDDocumentBuilder_AddPublicKey(builder, id1, did, keybase) == 0)
  printf("add 'test1' key successfully.");

if（DIDDocumentBuilder_AddAuthenticationKey(builder, id1, NULL) == 0)
  printf("add 'test1' key to authentication key successfully.");

... ... ... ...
DIDDocument *newDoc = DIDDocumentBuilder_Seal(builder, storePass);
DIDDocumentBuilder_Destroy(builder);
DIDURL_Destroy(id1);
... ... ... ...
```

## Usage

The newDid method is introduced in the chapter of Root Identity, which will not be explained here.

```c
DIDDocumentBuilder* DIDDocument_Edit(
    DIDDocument *document,
    DIDDocument *controllerdoc);
```

The method gets DIDDocumentBuilder object for changing documents.

The controllerdoc parameter is used to modify the customized DID Document and specify the controller for the first signature. For primitive DID Document, it's not necessary to give the controllerdoc.

```c
void DIDDocumentBuilder_Destroy(DIDDocumentBuilder *builder);
```

This method provides a method to destroy DIDDocumentBuilder object.

```c
DIDDocument *DIDDocumentBuilder_Seal(
    DIDDocumentBuilder *builder,
    const char *storepass);
```

The method encapsulates and gets a new DID Document. storepass is the password of DID Store, which is used to sign the main content of the modified DID Document by the master key and private key.

```c
int DIDDocumentBuilder_AddPublicKey(
        DIDDocumentBuilder *builder,
        DIDURL *keyid,
        DID *controller,
        const char *key);
```

The method adds a public key, and an error will be reported if the keyid or key exists.

```c
int DIDDocumentBuilder_RemovePublicKey(
        DIDDocumentBuilder *builder,
        DIDURL *keyid,
        bool force);
```

**Attention:** the default key cannot be removed/ When the Key to be removed is the Authentication or Authorization key, it's critical to decide whether to remove it according to the force parameter. If force is true, removal is supported; otherwise, an error will be reported.

```c
int DIDDocumentBuilder_AddAuthenticationKey(
        DIDDocumentBuilder *builder,
        DIDURL *keyid,
        const char *key);
```

The method adds an Authentication Key, and if the Key Id does not exist, a new Authentication Key will be generated; if the Key Id already exists, its content is the same as that of the existing one. In other words, if the Key Id is already an Authentication Key or an Authorization Key, it will be different, and an error will be reported.

If you want to add a known Key, you only need to provide a keyid, rather than a key. Key is the base58 string of the public key.

```c
int DIDDocumentBuilder_RemoveAuthenticationKey(
        DIDDocumentBuilder *builder,
        DIDURL *keyid);
```

This method removes the authentication key that already exists. If it doesn't exist, an error will be reported.

```c
int DIDDocumentBuilder_AddAuthorizationKey(
        DIDDocumentBuilder *builder,
        DIDURL *keyid,
        DID *controller,
        const char *key);
```

This method adds authorization Key according to the provided key content, which is only applicable to primitive DID Document. Generate a new Authorization Key, if the Key Id does not exist; if the Key Id already exists, the content of the Key is the same as that of the existing key. Otherwise, if the Key Id is already an Authentication Key or an Authorization Key, it will be different, and an error will be reported.

```c
int DIDDocumentBuilder_AuthorizeDid(
        DIDDocumentBuilder *builder,
        DIDURL *keyid,
        DID *controller,
        DIDURL *authorkeyid);
```

This method adds a new Authorization Key according to the provided controller and Key Id of controller, and if it already exists, an error will be reported.



```c
int DIDDocumentBuilder_RemoveAuthorizationKey(
        DIDDocumentBuilder *builder,
        DIDURL *keyid);
```

This method removes the specified Authorization Key. If the Key does not exist or is not an Authorization Key, an error will be reported.

```c
int DIDDocumentBuilder_AddCredential(
        DIDDocumentBuilder *builder,
        Credential *credential);
```

This method is used to add the Verifiable Credential provided by the user. If the Credential Id already exists in DID Document, an error will be reported.

```c
typedef struct Property {
    char *key;
    char *value;
} Property;

int DIDDocumentBuilder_AddSelfProclaimedCredential(
        DIDDocumentBuilder *builder,
        DIDURL *credid,
        const char **types, size_t typesize,
        Property *properties, int propsize,
        time_t expires,
        DIDURL *signkey,
        const char *storepass);
```

This method is used to directly generate and add self-declared credentials.

types are arrays of type, and typesize is the number of types; properties are arrays of credential subjects, which is the most important content of the credential, and propsize is the number of credential subjects.

At present, the SDK supports five types: Self-Proclaimed Credential, Email Credential, Profile Credential, Social Credential, and Wallet Credential, which will be adjusted as required. Users can choose the type according to their needs, or they can add customized types by corresponding methods.

expires is the effective period and cannot be greater than the effective period of the Document of the DID.

```c
int DIDDocumentBuilder_RemoveCredential(DIDDocumentBuilder *builder,
        DIDURL *credid);
```

This method removes the specified Credential. If it doesn't exist, an error will be reported.

```c
int DIDDocumentBuilder_AddService(
        DIDDocumentBuilder *builder,
        DIDURL *serviceid,
        const char *type,
        const char *endpoint,
        Property *properties, int size);
```

```c
int DIDDocumentBuilder_AddServiceByString(
        DIDDocumentBuilder *builder,
        DIDURL *serviceid,
        const char *type,
        const char *endpoint,
        const char *properties);
```

The above two methods are used to add Service, but the ways of importing attributes are different. If the specified id already exists, an error will be reported.

endpoint is the Service point address; properties are the content that users can add as their needs.

```c
int DIDDocumentBuilder_RemoveService(
        DIDDocumentBuilder *builder,
        DIDURL *serviceid);
```

This method removes the specified Service, and if it doesexn't ist, an error will be reported.

```c
int DIDDocumentBuilder_SetExpires(
        DIDDocumentBuilder *builder,
        time_t expires);
```

The effective period of DID Document defaults to five years after its creation. The user can define the effective period through this method. The effective period of this method setting cannot exceed five years, otherwise an error will be reported.
