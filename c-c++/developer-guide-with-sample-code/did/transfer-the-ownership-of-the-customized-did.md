# Transfer the ownership of the customized DID

前面已经介绍了Publish DID是用于普通DID和非更改持有者信息（Controller和Multisig）的Customized DID有效上链。这一小节就是介绍更改持有者信息后DID Document上链的使用说明。

DID Document提供TransferDID方法来完成更改持有者信息的交易，其需要同时基于修改后的文档和转移凭证（Transfer Ticket），使用DID原持有者主密钥对来完成该交易。

## Example

```c
//customized document has controller1 and controller2, multisig is 1:2
//customized document has Authentication Key 'key1'
//add new controller3 and set multisig is 2:3
DIDDocumentBuilder *builder = DIDDocument_Edit(doc, controller2);
if (!builder)
  //errer operation
  
if (DIDDocumentBuilder_AddController(builder, controller3) < 0 ||
   			DIDDocumentBuilder_SetMultisig(builder, 2) < 0)
  //error operation

DIDDocument *newDoc = DIDDocumentBuilder_Seal(builder, storepass);
DIDDocumentBuilder_Destroy(builder);
if(!newDoc)
  //error operation
  
const char *docData = DIDDocument_ToJson(newDoc, true);
DIDDocument_Destroy(newDoc);

newDoc = DIDDocument_SignDIDDocument(constrollerDoc3, docData, storepass);
free((void*)docData);
  
//create transfer ticket, mulitisig is 1:2 not 2:3
TransferTicket *ticket = DIDDocument_CreateTransferTicket(controller1Doc, did, controller3, storepass);
if (!ticket)
   //error operation
 
//transfer DID
if (DIDDocument_TransferDID(newDoc, ticket, key1, storepass) < 0)
  //error operation
  
TransferTicket_Destroy(ticket);
DIDDocument_Destroy(newDoc);
```

## Usage

```c
TransferTicket *DIDDocument_CreateTransferTicket(
        DIDDocument *controllerdoc,
        DID *owner, DID *to,
        const char *storepass);
```

DID Document提供生成Transfer Ticket的方法，其由修改前DID Document的一位Controller发起，Ticket也由该发起者签名封装。

`owner`是Transfer Ticket的所有者，就是Customized DID；`to`是Transfer Ticket的接受者，必须是修改后DID Document里的Controller之一，且为修改后DID Document的签名者之一。否则会导致后续Transfer DID失败。

```c
int DIDDocument_SignTransferTicket(
        DIDDocument *controllerdoc,
        TransferTicket *ticket,
        const char *storepass);
```

Transfer Ticket也需要符合修改前DID Document的多签规则，当第一个Controller签名生成Transfer Ticket后，若还需要多位Controller签名则使用该方法，具体用法可参考`DIDDocument_SignDIDDocument`方法。

```c
int DIDDocument_TransferDID(
        DIDDocument *document,
        TransferTicket *ticket,
        DIDURL *signkey,
        const char *storepass);
```

该方法用于Customized DID Document更改持有者信息后的上链，可由现`document`的Authentication Key 和其Controller 的Authentication Key来签名实现上链。
