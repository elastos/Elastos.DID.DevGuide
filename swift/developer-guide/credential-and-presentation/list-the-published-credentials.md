# List the Published Credentials

A specific DID can declare multiple credentials, and the ID sidechain also provides the features of retrieving all declared credentials of a specific DID. The DID SDK provides the listing method. For example:

```
let did = try DID("did:elastos:ihnk95mguTQzRrRrxFmc72z3r3y5NwKSJX")
// list all declared credentials for did
let vcIds = try VerifiableCredential.list(did)
```

If the declared credential is revoked, it will not be included in the resulting list.

If a DID has declared many credentials, only the most recent 128 credentials will be included in the default resulting list. To get all the credentials, paging support will be needed. Here is an example of the paginated list:

```
let did = try DID("did:elastos:ihnk95mguTQzRrRrxFmc72z3r3y5NwKSJX")
var skip = 0
let limit = 256
var ids: [DIDURL] = [ ]
while (true) {
    ids = try VerifiableCredential.list(did, skip, limit)
    if ids.count == 0 {
        break
		}
     // process the list result...
     skip += ids.count
}
```

Skip and limit are two parameters for paging, which respectively represent how many previous IDs are skipped and included in the result. The maximum implementation limit of Java SDK is 512.
