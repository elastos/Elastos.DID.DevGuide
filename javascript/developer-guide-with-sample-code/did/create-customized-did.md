# Create Customized DID

DIDSDK supports customized DID, which means that the corresponding DID document is needed. There is a cryptographically verifiable key between the subject and the default key in ordinary DID document. When the DID document finally uses this default key to sign the document body, there is a closed-loop verification process to avoid malicious means such as tampering.

However, the customized DID identifier is only an arbitrary string provided by the user, and it's impossible to generate a key cryptographically associated with the DID identifier. If a random key is added and used to sign the principal data in the DID document, then the key can be replaced by the content modified by others at any time, which cannot guarantee that the DID document will not be tampered with maliciously. To solve this problem, the controller and Multisig are introduced. The controller is the actual holder of the customized DID document, and Multisig refers to the multi-signature rule for multiple controllers.

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

This method is the most classic method to generate single-signed customized DID document. See the above example for details.

inputDID denotes the customized DID identifier.

Force indicates that if the document exists, whether to overwrite it or not. If true, the document is overwritten; if false, the document is not overwritten.

The newCustomizedDidWithController method in the example is more often used in multi-signed cases, so the detailed introduction is presented in the next section.

```typescript
public isCustomizedDid(): boolean；
```

This method can be used to check if the DID document is a customized one.
