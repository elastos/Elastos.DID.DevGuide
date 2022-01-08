# Publish DID

## Publish DID

Cochain can be divided into the effective and ineffective cochain. The ineffective cochain means that the cochain is informed that the existing DID has failed and cannot be used (this part will be introduced in the following section). The effective cochain means updating the effective DID information to the chain. This section introduces the instructions of this part.

DID Document stands for "DID cochain exposure," which provides Publish DID as a method of cochain. This method is suitable for an effective cochain of primitive and customized DID without changing the controllers’ information (Controller and Multisig).

In order to prevent the cochain content from being maliciously tampered with, DID signs the cochian content through its own Authentication Key, and the receiver verifies the signature of the content according to the provided information to confirm the reliability of the content.

## Example

```c
const char *rootPath = "root/store";
DIDStore *store = DIDStore_Open(rootPath);
const char *storePass = "pwd";
... ... ... ...
const char *ri = DIDStore_GetDefaultRootIdentity(store);
RootIdentity *rootidentity = DIDStore_LoadRootidentity(ri);
free((void*)ri);
DIDDocument *controllerDoc = RootIdentity_NewDID(rootidentity, storepass, "", true);
RootIdentity_Destroy(rootidentity);
//publish controller
if (DIDDocument_PublishDID(controllerDoc, NULL, true, storePass) == 0)
  	printf("publish controller successfully.\n");

// Create customized DID
DIDDocument *customizedDoc = DIDDocument_NewCustomizedDID(controllerDoc,
        "did:elastos:helloworld", NULL, 0, 0, true, storepass);

//publish DID
if (DIDDocument_PublishDID(customizedDoc, NULL, true, storePass) == 0)
  	printf("publish customized did successfully.\n");

... ... ... ...
DIDDocumentBuilder *db = DIDDocument_Edit(customizedDoc, DIDDocument_GetSubject(controllerDoc));
DIDDocument_Destroy(controllerDoc);
DIDDocument_Destroy(customizedDoc);
//add elem to document
... ... ... ... 
//seal DID Document
customizedDoc = DIDDocumentBuilder_Seal(db, storePass);
DIDDocumentBuilder_Destroy(db);
if (DIDDocument_PublishDID(customizedDoc, NULL, true, storePass) == 0)
  	printf("publish customized did successfully.\n");

... ... ... ...
DIDDocument_Destroy(customizedDoc);
DIDStore_Close(store);
```

## Usage

```c
int DIDDocument_PublishDID(
        DIDDocument *document,
        DIDURL *signkey,
        bool force,
        const char *storepass);
```

signkey is the Authentication Key designated to sign the cochain content. Here, it should be noted that any Authentication Key can be used to the primitive DID cochain; The sign key of Customized DID can only be its own Authentication Key or the Default Key of the Controller. When signkey is NULL, the Default Key signature of DID Document is used for cochain. Primitive DID must have a Default Key, but for Customized DID, only the Customized DID with single signature has a Default Key, and the Customized DID with multiple signatures does not. Therefore, if multi-signed Customized DID uses the default value NULL, an error will be reported. Please be sure to specify Sign Key.

force indicates whether the action of cochain can be done forcedly when DID Document expires or the local DID Document is not modified and updated based on the latest version on the chain. Generally, it's recommended that the local DID Document be modified based on the latest version on the chain, so as to be on-chain smoothly. Under special circumstances, setting force to true can also complete the action of cochain forcedly. When force is true, the documents expired can be updated.

**Attention:** For Customized DID Document, whether it is to change the Controller or multisig, the action of the cochain cannot be completed by Publish DID.
