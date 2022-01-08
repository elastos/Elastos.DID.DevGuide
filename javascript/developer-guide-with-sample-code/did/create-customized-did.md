# Create customized DID

DIDSDK 支持自定义DID，那么也就需要有对应的DID Document。普通DID Document里的Subject和Default Key之间存在密码学可验证的关键，DID Document最后用这个Default Key来签名Document主体是有个闭环的验证过程，可以避免被篡改等恶意手段。

DIDSDK supports customized DID, which means that the corresponding DID document is needed. There is a cryptographically verifiable key between the subject and the default key in ordinary DID document. When the DID document finally uses this default key to sign the document body, there is a closed-loop verification process to avoid malicious means such as tampering.

但是自定义DID标识只是用户提供的任意字符串，没法生成和DID标识有密码学相关联的Key。如果只是添加随意的Key，用该Key签名DID Document主体数据，那么随时都可以被他人修改内容替换Key，无法保证DID Document的不被恶意篡改。为了解决这个问题，引入了Controller和Multisig。Controller即为Customized DID Document的实际持有者，Multisig是多个Controller的多签规则。

However, the customized DID identifier is only an arbitrary string provided by the user, and it is impossible to generate a key cryptographically associated with the DID identifier. If a random key is added and used to sign the principal data in the DID document, then the key can be replaced by the content modified by others at any time, which cannot guarantee that the DID document will not be tampered with maliciously. To solve this problem, the controller and Multisig are introduced. The controller is the actual holder of the customized DID document, and Multisig refers to the multi-signature rule for multiple controllers.

根据controller的个数，Create Customized DID可以分为单签和多签两个情况，本小节提供两种单签的示例。

According to the number of controllers, the creation of customized DID involves two cases, namely single-signed and multi-signed. This section provides two single-signed examples.

## Example

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

## Usage

```typescript
public newCustomized(
	inputDID: DID | string,
	storepass: string,
	force?: boolean
): Promise<DIDDocument>；
```

该方法是最经典的生成单签的Customized DID Document的方法。具体使用可以看上面的示例。

This method is the most classic method to generate single-signed customized DID document. See the above example for details.

inputDID为自定义DID标识。

inputDID denotes the customized DID identifier.

force表示Document存在，是否覆盖。若为true，则覆盖；反之不覆盖。

Force indicates that if the document exists, whether to overwrite it or not. If true, the document is overwritten; if false, the document is not overwritten.

示例中newCustomizedDidWithController方法更多的用于多签，所以具体的介绍放在下一小节。

The newCustomizedDidWithController method in the example is more often used in multi-signed cases, so the detailed introduction is presented in the next section.

```typescript
public isCustomizedDid(): boolean；
```

该方法可以用来检查DID Document是否为Customized DID Document。

This method can be used to check if the DID document is a customized DID document.
