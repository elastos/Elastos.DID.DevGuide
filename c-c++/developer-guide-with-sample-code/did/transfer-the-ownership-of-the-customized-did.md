# Transfer Ownership of the Customized DID

It's already been mentioned that Publish DID is an effective cochain of Customized DID for primitive DID and non-change controller information (Controller and Multisig). This section is to introduce the instructions for using DID Document cochain after changing the information of controller.

DID Document provides the TransferDID method to complete the transaction of changing the controller information, which needs to use the master key of original controller of the DID to complete the transaction based on the modified document and the Transfer Ticket.

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

DID Document provides a method to generate Transfer Ticket, which is initiated by a Controller of DID Document before modification, and Tickets are also signed and encapsulated by the initiator.

owner is the owner of Transfer Ticket, that is, Customized DID; to is the recipient of the Transfer Ticket and must be one of the Controller in the modified DID Document and one of the signers of the modified DID Document. Otherwise, subsequent Transfer DID will fail.

```c
int DIDDocument_SignTransferTicket(
        DIDDocument *controllerdoc,
        TransferTicket *ticket,
        const char *storepass);
```

Transfer Ticket also needs to comply with the multi-signature rule of DID Document before modification. When the first Controller signs to generate Transfer Ticket, this method should be used if more than one signature of the Controller is needed. Refer to DIDDocument\_SignDIDDocument method for specific usage.

```c
int DIDDocument_TransferDID(
        DIDDocument *document,
        TransferTicket *ticket,
        DIDURL *signkey,
        const char *storepass);
```

This method is used for the cochain of Customized DID Document after changing the controller information, which can be signed by the Authentication Key of the current document and the Authentication Key of its Controller.
