# Verify the Integrity of DIDDocument

DIDDocument is a sealed object. Once the DID controller generates and signs the document, it can't be modified by any third party - any modification will destroy its integrity. Hence, DIDDocument object provides a series of methods to be verified, which can test the DID or its document in the following aspects:

* expiration
* deactivate
* genuine

## Expiration

When created, the DID has a validity period which is recorded in the corresponding DIDDocument. The DID represents a valid identity only within its validity period. The following methods can be used to verify whether a DID is within its validity period:

```
let did = try DID("did:elastos:icAGJstuDdRBRmx6NomZsdXLfZcAt1ANoV")
let doc = try did.resolve()
let expired = doc.isExpired
```

## Deactivate

The DID can be deactivated, which means it would be an identity that cannot be used or accepted by a third party. You can test whether a DID is deactivated using the following methods:

```
let did = try DID("did:elastos:icAGJstuDdRBRmx6NomZsdXLfZcAt1ANoV")
let doc = try did.resolve()
let deactivated = doc.isDeactivated
```

## Genuine

After obtaining a DIDDocument object, it's necessary to test whether the document has been maliciously modified, which is realized by the controller of DID signing the DID document itself. The test method is as follows:

```
let did = try DID("did:elastos:icAGJstuDdRBRmx6NomZsdXLfZcAt1ANoV")
let doc = try did.resolve()
let genuine = doc.isGenuine()
```

## Through Verification

In daily use, to verify whether a DID is effective, it's necessary to verify the above three aspects to obtain accurate results. The DID SDK provides a composite method, isValid (), which contains all the verifications. For example:

```
let did = try DID("did:elastos:icAGJstuDdRBRmx6NomZsdXLfZcAt1ANoV")
let doc = try did.resolve()
let valid = try doc.isValid()
```

> The verification method of the customized DID is the same as that of the ordinary DID.
