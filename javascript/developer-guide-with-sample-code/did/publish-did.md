# Publish DID

The publishing of DID is divided into valid and deactivated publishing. Deactivated publishing is to inform that the DID on the chain has failed and cannot be used again (this part will be introduced in the following section); valid publishing means updating the effective DID information to the chain (relevant instructions will be introduced in this section).

The DID Document stands for the publishing of DID, which provides Publish DID as a method of updating DID to the chain. This method is suitable for effectively updating ordinary DID and customized DID without changing the subject’s information (Controller and Multisig) to the chain.

To prevent the published content from being maliciously tampered with, DID signs the published content with its own authentication key, and the receiver verifies the signature of the content based on the provided information to confirm the reliability of the published content.

## Example

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
let storePass = "pwd";
... ... ... ...
let rootidentity = store.loadRootidentity();
let controller = await identity.newDid(storePass);
//publish controller
await controller.publish(storePass);
// Create customized DID
let did = new DID("did:elastos:helloworld");
//new Customized DID
let doc =  await controller.newCustomized(did, 1, storePass, false);
//publish DID
await doc.publish(storePass);
... ... ... ...
let db = DIDDocument.Builder.newFromDocument(doc).edit();
//add authentication key
let id = DIDURL.from("#test1", db.getSubject());
let key = HDKey.deriveWithPath(HDKey.DERIVE_PATH_PREFIX + 5);
db.addAuthenticationKey(id, doc.getSubject(), key.getPublicKeyBase58());
//seal DID Document
let doc = await db.seal(storePass);
doc.publish(storePass);
... ... ... ...
store.close();
```

## Usage

```typescript
export interface DIDTransactionAdapter {
	createIdTransaction(payload: string, memo: string);
}

public async publish(
	storepass: string,
	inputSignKey: DIDURL | string = null,
	force = false,
	adapter: DIDTransactionAdapter = null
): Promise<void>；
```

InputSignKey is the authentication key designated to sign the content updated to the chain. Here, it should be noted that the publishing of ordinary DID only requires a random authentication key. However, the sign key of customized DID can only be its own authentication key or the default key of the controller. When the inputSignKey is null, the default key of the DID document is used for signing the published content, and the ordinary DID document must have the default key. However, for customized DIDs, only the customized DID with single signature has the default key, while the multi-signed customized DID does not have one. Therefore, if the multi-signed customized DID uses the default value null, an error will be reported. Please be sure to assign the specified sign key.

Force indicates whether it's possible to force the publishing of data to the chain when the DID document expires or the local DID document is not modified and updated based on the latest version on the chain. Generally, it's recommended that the local DID Document be modified based on the latest version on the chain, so as to smoothly publish the document. Under special circumstances, by setting force to be true, the publishing can also be forced. If force is true, the documents that have expired can also be updated.

The adapter provides an interface for publishing the data to the chain. Users can implement it on their own or use the default interface provided by Publish DID.

N**ote:** Whether by modifying the controller or multisig, the customized DID document cannot publish the data to the chain by means of Publish DID.
