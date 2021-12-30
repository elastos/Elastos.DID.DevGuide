# List the Published Credentials

A specific DID can expose multiple credentials, and the ID sidechain also provides the feature of retrieving all public credentials of a specific DID. The DID SDK provides the list method. For example:

```java
DID did = new DID("did:elastos:ihnk95mguTQzRrRrxFmc72z3r3y5NwKSJX");

// list all declared credentials for did
List<DIDURL> vcIds = VerifiableCredential.list(did);
```

If the published credential is revoked, it will not be included in the result list.

If a DID has many public credentials, the default list results only contain the most recent 128. Paging support will be used if all the credentials need to be gotten. An example of paging list is as follows:

```java
DID did = new DID("did:elastos:ihnk95mguTQzRrRrxFmc72z3r3y5NwKSJX");

int skip = 0;
int limit = 256;
List<DIDURL> ids;
while (true) {
    ids = VerifiableCredential.list(did, skip, limit);
    if (ids == null)
        break;

     // process the list result...

     skip += ids.size();
}
```

Skip and limit are two parameters for paging, which respectively represent how many IDs are skipped and how many IDs are included in the result. The maximum implementation limit of Java SDK is 512.
