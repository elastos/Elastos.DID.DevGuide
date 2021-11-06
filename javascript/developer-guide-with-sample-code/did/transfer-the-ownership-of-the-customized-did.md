# Transfer the ownership of the customized DID
前面已经介绍了Publish DID是用于普通DID和非更改持有者信息（Controller和Multisig）的Customized DID有效上链。这一小节就是介绍更改持有者信息后DID Document上链的使用说明。

DID Document提供TransferDID方法来完成更改持有者信息的交易，其需要同时基于修改后的文档和转移凭证（ticket），使用DID原持有者主密钥对来完成该交易。

# Example
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

# Usage
```typescript
public async createTransferTicket(
	to: DID,
	storepass: string,
	from?: DID
): Promise<TransferTicket>;
```
DID Document提供生成Transfer Ticket的方法。createTransferTicket有修改前DID Document的其中一位Controller发起，Ticket也有该发起者签名封装。

to是Transfer Ticket的接受者，必须是修改后DID Document里的Controller之一，修改后DID Document的签名者之一。否则Transfer DID失败。

from是Transfer Ticket的归属者，就是Customized DID。

```typescript
public async publishWithTicket(
	ticket: TransferTicket,
	inputSignKey: DIDURL | string | null,
	storepass: string,
	adapter: DIDTransactionAdapter = null
): void；
```
inputSignKey为修改后文档的Authentication Key或者Controller的主Key。其他参数同Publish DID。




