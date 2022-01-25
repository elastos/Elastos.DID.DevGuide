# Deactivate DID

If the DID is no longer used or needed, it can be deactivated. After that, the DID will be in an invalid state and cannot be used for authentication and any other DID-related operations.

The deactivating operation of DID typically contains two situations:

* The controller of DID actively deactivates DID
* The private key is lost, but the DID declares an authorization Key, which can be deactivated by the principal

The results of deactivating DID through these two forms are the same, which will directly lead to the permanent invalidation of DID.

## DID is Deactivated by Controller

The DID controller can deactivate the DID with the default authentication key corresponding to it, and only with the default authentication key can the deactivating operation be launched. An example is as follows:

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID did = new DID("did:elastos:ig5M4ro4JCWTdkeNE5tVntgGQAtU6o1bL9");

// Get the existing DIDDocument
DIDDocument doc = store.loadDid(did);

// Deactivate DID
doc.deactivate(storePasswd);
```

For customized DID, deactivating the DID can be launched by any valid controller. Similarly, the deactivating operation needs to be signed by the controller’s default authentication key. For example:

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
// foobar controlled by ig5M4ro4JCWTdkeNE5tVntgGQAtU6o1bL9
// and the store has the default private key for ig5M4ro4JCWTdkeNE5tVntgGQAtU6o1bL9
DID did = new DID("did:elastos:ig5M4ro4JCWTdkeNE5tVntgGQAtU6o1bL9");
DID foobar = new DID("did:elastos:foobar");

// Get the existing DIDDocument
DIDDocument foobarDoc = store.loadDid(foobar);
// ig5M4ro4JCWTdkeNE5tVntgGQAtU6o1bL9 on beharf of foobar
foobarDoc.setEffectiveController(did);

// Deactivate foobar by ig5M4ro4JCWTdkeNE5tVntgGQAtU6o1bL9
foobarDoc.deactivate(storePasswd);
```

## The Principal Deactivates the DID

This method is only applicable to primitive DID, and isn't necessary to set a principal for customized DID because of the controller.

For security reasons, primitive DID can set one or more trusted principals, which is embodied by specifying the trusted entrustment key. Based on the principle of minimum authorization, this delegation key can only be used to deactivate DID. The goal is that after the DID controller loses the key, the principal can invalidate the DID, thus reducing the potential safety hazard caused by the key loss.

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
// did has a authorization key from delegatee's key #abc
DID did = new DID("did:elastos:ig5M4ro4JCWTdkeNE5tVntgGQAtU6o1bL9");
// and the store has the private key for delegatee's key #abc
DID delegatee = new DID("did:elastos:inxkMjrXt9zF1s1aoJFq1dja85C9Mm8wm6");
DID keyId = new DIDURL("did:elastos:inxkMjrXt9zF1s1aoJFq1dja85C9Mm8wm6#abc");

// Get the delegatee's DIDDocument
DIDDocument delegateeDoc = store.loadDid(delegatee);

// Deactivate foobar by ig5M4ro4JCWTdkeNE5tVntgGQAtU6o1bL9
delegateeDoc.deactivate(did, keyId, storePasswd);
```
