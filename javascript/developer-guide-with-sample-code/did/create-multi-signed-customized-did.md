# Create Multi-signed Customized DID

This section mainly introduces the method of generating and modifying the multi-signed customized DID document, which isn't available in the ordinary document.

The multi-signature (m:n) rule requires n>1. M represents the number of signers, that is, the number of signatures in the proof of document. If the number of signatures is less than m, it means that the document at this time is not really a valid multi-signed document, and the remaining controllers who haven’t signed need to sign the documents.

## Example

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

## Usage

The methods of adding and removing elements in the ordinary DID document are also applicable to the customized DID document, and what is described here is the unique content of customized DID document.

```typescript
public async newCustomizedDidWithController(
	inputDID: DID | string,
	inputControllers: Array<DID | string>,
	multisig: number,
	storepass: string,
	force?: boolean
): Promise<DIDDocument>；
```

This method generates the initial multi-signed document. At this time, only one controller signs the primary data in the obtained document, and whether the number of signatures accords with the multi-signature rule is verified by the isQualified method.

```typescript
public isQualified(): boolean;
```

This method tells whether the number of signatures in the current customized document lives up to the multi-signature rule. If false is returned, the controller who hasn’t signed will continue to complete the signature work to improve the document.

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

The above two methods are apparently exclusive to the customized DID document. If adding and removing controllers is used for the ordinary DID document, an error is returned.

```typescript
public setMultiSignature(
    m: number
): Builder;
```

This method resets the over-signature rule.

```typescript
public setEffectiveController(controller: DID)：void;
```

Before signing the multi-signed customized DID document, the effective controller should set to specify which controller’s master key is used for signing.
