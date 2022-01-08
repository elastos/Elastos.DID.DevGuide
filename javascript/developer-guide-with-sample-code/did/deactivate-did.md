# Deactivate DID

Both Publish and Transfer DID update valid data to the chain, while Deactivate DID means stopping the use of the DID.

The DID can be deactivated by itself or the client. The ordinary DID can be deactivated by the authentication key and the authorization key. The customized DID can be deactivated by the authentication key and the controller’s default key.

The DID that has not been published to the chain cannot be deactivated.

## Example

Deactivate self use authentication key：

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
let storePass = "pwd";
... ... ... ...
let rootidentity = store.loadRootidentity();
let doc = await identity.newDid(storePass);
//publish controller
await doc.publish(storePass);
.. ... ... ...
let resolved = await doc.getSubject().resolve();
if (resolved)
	await doc.deactivate(null, storePass, null);
if (doc.isDeactivated())
	console.log("deactivate did successfully.");
else
	console.log("deactivate did failed.");
... ... ... ...
store.close();
```

Deactivate target DID by the authorizor's DID：

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
let storePass = "pwd";
... ... ... ...
let rootidentity = store.loadRootidentity();
let doc =  await identity.newDid(storePass);
let db = DIDDocument.Builder.newFromDocument(doc).edit();

let id =  DIDURL.from("#key-2", doc.getSubject());
let key = HDKey.deriveWithPath(HDKey.DERIVE_PATH_PREFIX  +  5);
db.addAuthenticationKey(id, key.getPublicKeyBase58());
store.storePrivateKey(id, key.serialize(), storePass);
doc = await db.seal(storePass);
await store.storeDid(doc);
await doc.publish(storePass);
let resolved =  await doc.getSubject().resolve();
... ... ... ...
let target = await identity.newDid(storePass);
db = DIDDocument.Builder.newFromDocument(target).edit();
db.addAuthorizationKey("#recovery", doc.getSubject().toString(), key.getPublicKeyBase58());
target =  await db.seal(storePass);
await store.storeDid(target);
await target.publish(TestConfig.storePass);
resolved =  await target.getSubject().resolve();
if (resolved)
	console.log();

await doc.deactivateTargetDID(target.getSubject(), null, storePass, null);
target =  await target.getSubject().resolve();
if (target.isDeactivated())
	console.log("deactivate did successfully.");
else
	console.log("deactivate did failed.");
... ... ... ...
store.close();
```

## Usage

```typescript
public async deactivate(
	signKey: DIDURL = null,
	storepass: string,
	adapter: DIDTransactionAdapter = null
): void；
```

This method is initiated by the deactivated DID itself. The DID deactivates does this using its own authentication key.

SignKey is the authentication key designated to sign the deactivated transaction. If signKey is null, the default key will be used by default.

The other parameters are the same as those of the publish DID method.

```typescript
public async deactivateTargetDID(
	target: DID,
	signKey: DIDURL = null,
	storepass: string,
	adapter: DIDTransactionAdapter = null
): void；
```

This method is a deactivation operation initiated by the client.&#x20;

The target is the DID to be deactivated.

SignKey: The deactivation of ordinary DID is initiated by the authorizer DID document, which provides the authorizer authentication key. This key must exist as authorization key in the DID document of the target. The customized DID is initiated by the controller DID document, with the controller default key as the sign key.

The other parameters are the same as those of Publish DID.
