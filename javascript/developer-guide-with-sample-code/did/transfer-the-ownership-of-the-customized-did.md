# Transfer Ownership of the Customized DID

As mentioned in the previous section, Publish DID is used for effectively updating ordinary DID and customized DID without changing the subject’s information (Controller and Multisig) to the chain. This section introduces how to update the DID document to the chain after modifying the information of the controller.

The DID document offers the TransferDID method to complete the transaction of modifying the controller’s information. This transaction should be accomplished using the master key of the original controller of the DID based on the modified document and the transfer ticket.

## Example

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
let storePass = "pwd";
... ... ... ...
let identity = store.loadRootidentity();
// Create normal DID first
let controller = await identity.newDid(storePass);
await controller.publish(storePass);
// Create customized DID
let did = new DID("did:elastos:helloworld");
let doc = await controller.newCustomized(did, 1, storePass);
await doc.publish(storePass);
// create new controller
let newController = await identity.newDid(storePass);
await newController.publish(storePass);
// create the transfer ticket from one old Controller
let ticket = await controller.createTransferTicket(newController.getSubject(), storePass, doc.getSubject());
// create new document for customized DID
doc = await newController.newCustomized(did, 1, storePass, true);
// transfer DID
await doc.publishWithTicket(ticket, newController.getDefaultPublicKeyId(), storePass);
... ... ... ...
store.close();
```

## Usage

```typescript
public async createTransferTicket(
	to: DID,
	storepass: string,
	from?: DID
): Promise<TransferTicket>;
```

The DID document provides the method of generating the transfer ticket. CreateTransferTicket was initiated by one of the controllers of the DID document before it's modified, and the transfer ticket is also signed and encapsulated by the initiator.

To is the recipient of the transfer ticket, who must be one of the controllers and signers of the modified DID document - otherwise, the transfer DID fails.

From is the owner of the transfer ticket - that is, the customized DID.

```typescript
public async publishWithTicket(
	ticket: TransferTicket,
	inputSignKey: DIDURL | string | null,
	storepass: string,
	adapter: DIDTransactionAdapter = null
): void；
```

inputSignKey is the authentication key of the modified document or the master key of the controller. The other parameters are the same as those of Publish DID.
