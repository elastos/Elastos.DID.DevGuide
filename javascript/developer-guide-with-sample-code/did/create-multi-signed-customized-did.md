# Create multi-signed Customized DID

本小节主要是介绍多签的Customized DID Document的生成方法和修改Customized DID Document中普通Document没有的方法。

多签（m:n）规则，只要n>1即为多签。m表示签名者个数，即Document 的Proof中signature个数。如果signature个数小于m则表明此时的Document还不是真正可用有效的多签文档，需要剩余未签名的Controller完成签名工作。

# Example
```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
let storePass = "pwd";
... ... ... ...
let rootidentity = store.loadRootidentity();
//get the first controller
let controller1 = await identity.newDid(storePass);
await controller1.publish(storePass);
//get the second controller
let controller2 = await identity.newDid(storePass);
await controller2.publish(storePass);
//get the third controller
let controller3 = await identity.newDid(storePass);
await controller3.publish(storePass);
... ... ... ...
let did = new DID("did:elastos:byeworld");
//new multi-signed Customized DID
//customized document has three controller and multisig is 2:3, so document must be signed by two controllers. controller2 is the first signer.
let doc = await controller2.newCustomizedDidWithController(did, [controller1.getSubject(), controller2.getSubject(), controller3.getSubject()], 2, storePass);
//check signer is enough
if (!doc.isQualified())
	//the second signer, and finished multi-signure.
	await controller1.signWithDocument(doc, storePass);

//check the doc
if (doc.isQualified()) {
	console.log("create multi-signed customized document successfully." ); 
} else {
	console.log("create multi-signed customized document failed." );
}
... ... ... ... 
//modify the document:remove controller1,mutisig 2:2
let db = DIDDocument.Builder.newFromDocument(doc).edit(controller2);
db.removeController(controller1.getSubject());
//do other things
... ... ... ...
let doc = db.seal();
await controller3.signWithDocument(doc, storePass);
if (doc.isValid()) {
	console.log("create multi-signed customized document successfully." ); 
} else {
	console.log("create multi-signed customized document failed." );
}

doc.setEffectiveController(controller3.getSubject());
await doc.publish(storePass);
... ... ... ...
store.close();
```
# Usage

普通DID Document的那些添加和移除元素的方法同样适合Customized DID Document，这里说明的是Customized DID Document特有的内容。

```typescript
public async newCustomizedDidWithController(
	inputDID: DID | string,
	inputControllers: Array<DID | string>,
	multisig: number,
	storepass: string,
	force?: boolean
): Promise<DIDDocument>；
```
该方法提供了生成初始的多签Document，此时得到的Document只有一个Controller对主体数据签名，此时signature个数是否符合多签规则，通过isQualified方法来提供。

```typescript
public isQualified(): boolean;
```
该方法告知当前的Customized Document中signature个数是否符合多签规则。如若返回false，则未签名的Controller继续完成签名工作，完善Document。

```typescript
public async addController(
    controller: DID | string
): Promise<Builder>；
```

```typescript
public removeController(
    controller: DID | string
): Builder;
```
上面两个方法很明显的是Customized DID Document才有的方法，添加和移除Controller，用于普通DID Document，则报错。

```typescript
public setMultiSignature(
    m: number
): Builder;
```
该方法重新设置了过签规则。

```typescript
public setEffectiveController(controller: DID)：void;
```
多签的Customised DID Document在签名前需要设置Effective Controller来指定签名是用哪个controller的主key。

