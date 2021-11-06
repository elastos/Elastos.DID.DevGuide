# Create customized DID
DIDSDK 支持自定义DID，那么也就需要有对应的DID Document。普通DID Document里的Subject和Default Key之间存在密码学可验证的关键，DID Document最后用这个Default Key来签名Document主体是有个闭环的验证过程，可以避免被篡改等恶意手段。

但是自定义DID标识只是用户提供的任意字符串，没法生成和DID标识有密码学相关联的Key。如果只是添加随意的Key，用该Key签名DID Document主体数据，那么随时都可以被他人修改内容替换Key，无法保证DID Document的不被恶意篡改。为了解决这个问题，引入了Controller和Multisig。Controller即为Customized DID Document的实际持有者，Multisig是多个Controller的多签规则。

根据controller的个数，Create Customized DID可以分为单签和多签两个情况，本小节提供两种单签的示例。

# Example
```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
let storePass = "pwd";
... ... ... ...
let rootidentity = store.loadRootidentity();
let controller = await identity.newDid(storePass);
await controller.publish(storePass);
... ... ... ...
resolved = await controller.getSubject().resolve();

// Create customized DID
let did = new DID("did:elastos:helloworld");
//new Customized DID
let doc =  await controller.newCustomized(did, 1, storePass, false);
await doc.publish(storePass);
... ... ... ...
let resolved =  await did.resolve();
... ... ... ...
store.close();
```
```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
let storePass = "pwd";
... ... ... ...
let rootidentity = store.loadRootidentity();
let controller = await identity.newDid(storePass);
await controller.publish(storePass);
... ... ... ...
resolved = await controller.getSubject().resolve();

let newDid = new DID("did:elastos:byeworld");
//two method to new Customized DID
let newDoc = await controller.newCustomizedDidWithController(newDid, [controller], 1, storePass);
await newDoc.publish(storePass);
... ... ... ...
let resolveDoc =  await newDid.resolve();
... ... ... ...
store.close();
```
# Usage

```typescript
public newCustomized(
	inputDID: DID | string,
	storepass: string,
	force?: boolean
): Promise<DIDDocument>；
```
该方法是最经典的生成单签的Customized DID Document的方法。具体使用可以看上面的示例。

inputDID为自定义DID标识。

force表示Document存在，是否覆盖。若为true，则覆盖；反之不覆盖。

示例中newCustomizedDidWithController方法更多的用于多签，所以具体的介绍放在下一小节。

```typescript
public isCustomizedDid(): boolean；
```
该方法可以用来检查DID Document是否为Customized DID Document。


