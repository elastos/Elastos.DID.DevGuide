# Deactivate DID

If the DID is out of use, it can be deactivated. The deactivated DID will be in an invalid state and cannot be used for authentication and any other DID-related operations.

Usually, the DID can be deactivated in the following two ways:

* The controller of DID takes the initiative to deactivate the DID
* If the DID controller loses the private key to this DID, this DID can be deactivated by the trustee with authorizationKey

The above two methods of deactivating DID will lead to the same result, or the permanent invalidation of DID.

## The DID Controller Deactivates DID

The DID controller can only deactivate it with the default authentication key corresponding to the DID. The example is as follows:

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let did = try DID("did:elastos:iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2")
// Get the existing DIDDocument
let doc = try store.loadDid(did)
// Deactivate DID
doc.deactivate(storePasswd)
```

Any valid controller can deactivate the customized DID. Likewise, the deactivate operation should be signed by the controller with the default authentication key. For example:

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
// ttech controlled by iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2
// and the store has the default private key for iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2
let did = try DID("did:elastos:iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2")
let ttech = try DID("did:elastos:ttech")
// Get the existing DIDDocument
let ttechDoc = try store.loadDid(ttech)
// iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2 on beharf of ttech
try ttechDoc.setEffectiveController(did)
// Deactivate ttech by iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2
try ttechDoc.deactivate(storePasswd)
```

## The Trustee Deactivates DID

This method is only applicable to the ordinary DID. The customized DID has its controllers, so there is no need for it to set any trustee.

For security reasons, the ordinary DID can set one or more trustees by specifying the trusted key. Based on the principle of minimum authorization, this key can only be used to deactivate the DID. If the DID controller loses the key, the trustee can reduce the potential safety hazard caused by the key loss by deactivating the DID.

```
var store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
// did has a authorization key from delegatee's key #abc
let did = try DID("did:elastos:iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2")
// and the store has the private key for delegatee's key #abc
let delegatee = try DID("did:elastos:inxkMjrXt9zF1s1aoJFq1dja85C9Mm8wm6")
let keyId = try DIDURL("did:elastos:inxkMjrXt9zF1s1aoJFq1dja85C9Mm8wm6#abc")
// Get the delegatee's DIDDocument
let delegateeDoc = try store.loadDid(delegatee)
// Deactivate foobar by iXyYFboFAd2d9VmfqSvppqg1XQxBtX9ea2
try delegateeDoc.deactivate(with: did, of: keyId, using: storePasswd)
```
