# Deactivate DID

DID 如果不再使用，可以对 DID 进行 deactivate，之后该 DID 将处于无效的状态，不能用于身份验证和其他任何 DID 相关的操作。

If the DID is no longer used, it can be deactivated. After that, the DID will be in an invalid state and cannot be used for authentication and any other DID-related operations.

DID 的 deactivate 操作通常有两种情形：

Deactivating operation of DID usually has two situations:

* DID 的持有者主动的对 DID 进行 deactivate
* The controller of DID actively deactivate DID
* DID 持有者遗失的该 DID 的私钥，但是该 DID 声明有委托密钥（authorizationKey），可以由被委托人代为 deactivate
* The private key is lost, but the DID declares an authorization Key, which can be deactivated by the principal.

通过这两种形式对 DID 进行 deactivate，其结果是相同的，都会直接导致该 DID 永久失效。

The results of deactivating the DID through these two forms are the same, which will directly lead to the permanent invalidation of DID.

## DID 持有者 deactivate DID

The DID is deactivated by controller

DID 持有者可以以持有的该 DID 对应的默认认证密钥，并且仅能使用默认认证密钥对 DID 进行 deactivate 操作。示例如下：

The DID controller can deactivate the DID with the default authentication key corresponding to the DID, and only with the default authentication key that the deactivating operation can be launched. Example is as follows:

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID did = new DID("did:elastos:ig5M4ro4JCWTdkeNE5tVntgGQAtU6o1bL9");

// Get the existing DIDDocument
DIDDocument doc = store.loadDid(did);

// Deactivate DID
doc.deactivate(storePasswd);
```

对于自定义 DID，deactivate 可以由任何一个有效的持有人（controller）对 DID 进行 deactivate，同样，deactivate 操作需要使用持有人的默认认证密钥进行签名。示例：

For customized DID, the deactivating of DID can be launched by any valid controller. Similarly, the deactivating operation needs to be signed by the controller’s default authentication key. Example:

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

## 被委托人代为 deactivate DID

The principal deactivates the DID

这个方式只适用于普通 DID，自定义 DID 因为有了 controller，没有设定委托人的必要。

This method is only applicable to primitive DID, and it is not necessary to set a principal for customized DID because of the controller.

普通 DID 出于安全的考虑，可以设定1个或者多个信任的委托人，体现形式就是指定信任的委托密钥。基于最小授权原则，该委托密钥仅能用于对 DID 进行 deactivate 操作。目标是在 DID 持有人遗失密钥后，可以由被委托人将 DID 失效，从而降低密钥遗失带来的安全隐患。

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
