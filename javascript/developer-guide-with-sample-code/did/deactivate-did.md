# Deactivate DID

Publish DID方法和Transfer DID方法都是有效上链，Deactivate DID即为失效上链，停用DID。

DID可以由自己或者委托者来停用DID，普通DID有Authentication Key和Authorization Key完成停用，自定义DID有Authentication Key和Controller的Default Key完成停用。

未上链过的DID不可执行Deactivate操作。

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

Deactivate target DID by authorizor's DID：

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

该方法是有被停用DID自己发起，用自己的Authentication Key完成停用。

signKey是被指定签名Deactivate交易的Authentication Key，signKey为null，则默认使用Default Key。

其他参数同Publish DID方法。

```typescript
public async deactivateTargetDID(
	target: DID,
	signKey: DIDURL = null,
	storepass: string,
	adapter: DIDTransactionAdapter = null
): void；
```

该方法是有委托者发起的停用操作。

target就是等待被停用的DID。

signKey：普通DID的停用就由Autherizor DID Document发起，提供Autherizor Authentication Key，该Key也必须在Target的DID Document作为Authorization Key存在；自定义DID就是用Controller DID Document发起，提供Controller Default Key作为Sign Key。

其他参数同Publish DID。


