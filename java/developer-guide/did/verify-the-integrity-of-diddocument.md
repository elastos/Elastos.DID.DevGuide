# Verify the Integrity of DID Document

DID Document is a sealed object. Once the DID controller generates and signs the document, it cannot be modified by any third party, and all modifications will destroy the integrity of DID Document. Therefore, DID Document object provides a set of methods to verify the object, which can test the following aspects of DID or document:

* Expiration
* Deactivate
* Genuine

## Expiration

A DID has an effective period when it's created, and the period is recorded in the corresponding DID Document. The DID is a valid identity only when it is within the effective period. For any DID, you can verify whether it's in the effective period by the following methods:

```java
DID did = new DID("did:elastos:icAGJstuDdRBRmx6NomZsdXLfZcAt1ANoV");
DIDDocument doc = did.resolve();
boolean expired = doc.isExpired();
```

## Deactivate

The DID can be revoked, and when this happen, it's an identity that cannot be used and accepted by a third party. You can test whether DID is revoked by the following methods:

```java
DID did = new DID("did:elastos:icAGJstuDdRBRmx6NomZsdXLfZcAt1ANoV");
DIDDocument doc = did.resolve();
boolean deactivated = doc.isDeactivated();
```

## Genuine

After getting a DID Document object, it's necessary to test whether the document has been maliciously modified, which is realized by the signing of the DID document by the controller. The test method is as follows:

```java
DID did = new DID("did:elastos:icAGJstuDdRBRmx6NomZsdXLfZcAt1ANoV");
DIDDocument doc = did.resolve();
boolean genuine = doc.isGenuine();
```

***

## Comprehensive Verification

In daily use, in order to verify whether a DID is valid and receive accurate results, the above three aspects need to be verified. The DID SDK provides a composite method isValid (), which contains all the verifications. For example:

```java
DID did = new DID("did:elastos:icAGJstuDdRBRmx6NomZsdXLfZcAt1ANoV");
DIDDocument doc = did.resolve();
boolean valid = doc.isValid();
```

> The verification method of customized DID is consistent with that of primitive DID.
