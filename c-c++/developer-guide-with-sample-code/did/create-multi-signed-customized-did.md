# Create a Multi-signed Customized DID

This section mainly introduces the generation method of a multi-signed Customized DID Document and method of modifying the Customized DID Document, which is not available in the primitive Document.

According to multi-signature(m:n) rule, as long as n>1, it is multi-signature. _m_ represents the number of signers, that is, the number of signature in the Proof of Document. If the number of signatures is less than m, it means that the Document at this time is not really a valid multi-signature document, and the remaining unsigned Controller are needed to complete the signature work.

## Example

```c
    DIDStore *store = DIDStore_Open("/DID Store");
    if (!store)
        return;
    ... ... ... ...
    //'littlefish' has only controller1
    DIDDocument *customizedDoc = DIDDocument_NewCustomizedDID(controller1Doc, "littlefish",
                NULL, 0, 0, false, storepass);
		DID *did = DIDDocument_GetSubject(customizedDoc);
    if (!customizedDoc)
      	goto errorExit;

    //edit 'littlefish'
    DIDDocumentBuilder *builder = DIDDocument_Edit(customizedDoc, controller2Doc);
		if (!builder)
      	goto errorExit;

    //add two controllers: controller2 and controller3
		if (DIDDocumentBuilder_AddController(builder, controller2) == -1 ||
       			DIDDocumentBuilder_AddController(builder, controller3) == -1) {
        DIDDocumentBuilder_Destroy(builder);
      	goto errorExit;
    }

		//add self-claimed credential signed by controller1
    DIDURL *credid = DIDURL_NewFromDid(did, "vc-2");
		if (!credid) {		
      	DIDDocument_Destroy(customizedDoc);
        DIDDocumentBuilder_Destroy(builder);
      	goto errorExit;
    }

    const char *types[] = {"BasicProfileCredential", "SelfProclaimedCredential"};
    Property props[2];
    props[0].key = "phone";
    props[0].value = "5737837";
    props[1].key = "address";
    props[1].value = "Shanghai";

    if (DIDDocumentBuilder_AddSelfProclaimedCredential(builder, credid,
            types, 2, props, 2, expires, NULL, storepass) == -1) {
      	DIDDocument_Destroy(customizedDoc);
        DIDDocumentBuilder_Destroy(builder);
      	goto errorExit;
    }

    //set multisig
		if (DIDDocumentBuilder_SetMultisig(builder, 2) == -1) {
      	DIDDocument_Destroy(customizedDoc);
        DIDDocumentBuilder_Destroy(builder);
      	goto errorExit;
    }

    //seal builder
    DIDDocument *newCustomizedDoc = DIDDocumentBuilder_Seal(builder, storePass);
		DIDDocumentBuilder_Destroy(builder);
		const char *docData = DIDDocument_ToJson(newCustomizedDoc, true);
		DIDDocument_Destroy(newCustomizedDoc);

		newCustomizedDoc = DIDDocument_SignDIDDocument(constrollerDoc2, docData, storepass);
		free((void*)docData);

		if (DIDDocument_IsQualified(newCustomizedDoc) != 1) {
        DIDDocument_Destroy(customizedDoc);
      	goto errorExit;
		}
      
    //edit new Customized Document
		builder = DIDDocument_Edit(newCustomizedDoc, controller2Doc);
		DIDDocument_Destroy(newCustomizedDoc);
		if (!builder)  {
      	DIDDocument_Destroy(customizedDoc);
      	goto errorExit;
    }

    //remove controller1, failed. Error: "There are self-proclaimed credentials signed by controller, please remove or renew these credentials at first."
		if (DIDDocumentBuilder_RemoveController(builder, controller1) == -1)
      	printf("%s\n", DIDError_GetLastErrorMessage());

		//renew self-claimed credential
    if (DIDDocumentBuilder_RenewSelfProclaimedCredential(builder, controller3, signkey2, storepass) == -1) {
      	DIDDocumentBuilder_Destroy(builder);
      	DIDDocument_Destroy(customizedDoc);
      	goto errorExit;
    }

		//remove controller1 succesfully
   	if (DIDDocumentBuilder_RemoveController(builder, controller1) == -1) { 
        DIDDocumentBuilder_Destroy(builder);
      	DIDDocument_Destroy(customizedDoc);
      	goto errorExit;      
    }

		newCustomizedDoc = DIDDocumentBuilder_Seal(builder, storePass);
		DIDDocumentBuilder_Destroy(builder);
		DIDDocument_Destroy(customizedDoc);

errorExit:
	DIDStore_Close(store);
  return;
```

## Usage

The methods of adding and removing elements of primitive DID Document are also suitable for Customized DID Document. Here, the unique content of Customized DID Document is described.

```c
DIDDocument *DIDDocument_SignDIDDocument(
        DIDDocument* controllerdoc,
        const char *document,
        const char *storepass);
```

This method is used for multi-signature. When the signature of Customized Document does not meet the multi-signature rule, other controllers need to use this method to sign the document.\\

When the first controller gets the document by DIDDocument\_NewCustomizedDID method, DID Document object can check whether the document meets the multi-signature rule by DIDDocument\_IsQualified; if not, other controllers use this method to continue to sign the document.

Document data which is continued to be signed is needed by the dicument.

```c
const char *DIDDocument_MergeDIDDocuments(int count, ...);
```

This method is used to merge multiple documents (whether the documents meet the multi-signature rule or not), remove the signatures of the same controller in the count document, and merge different signatures to unify the documents.

Example: controller1 initiates the creation of Customized Document, multi-signature rule 3:4, and generate doc1; Controller2 adds another signature to doc1 by DIDDocument\_SignDIDDocument to get doc2, and controller3 adds another signature to doc1 by DIDDocument\_SignDIDDocument to get doc3. At this time, the signatures of three controllers in the multi-signature rule are completed, but neither doc2 nor doc3 meets the multi-signature rule. The user can get the complete documents signed by controller1, controller2 and controller3 through DIDDocument\_MergeDIDDocuments.

```c
int DIDDocument_IsQualified(DIDDocument *document);
```

This method tells whether the number of signatures in the current Customized Document meets the multi-signature rule.

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, DID Document isn't qualified;
  ```
* ```
   return value = 1, DID Document is qualified.
  ```

```c
int DIDDocumentBuilder_AddController(
    DIDDocumentBuilder *builder,
    DID *controller);
```

```c
int DIDDocumentBuilder_RemoveController(
    DIDDocumentBuilder *builder,
    DID *controller);
```

The above two methods obviously belong to Customized DID Document. If the actions of adding and removing Controller are done to the primitive DID Document, an error will be reported.

It should be noted here that if DID Document has the Credential signed by the Controller that needs to be deleted, it cannot be deleted directly. The credential needs to be renewed or removed. If there is only the last controller left, it will also fail.

```c
int DIDDocumentBuilder_SetMultisig(
    DIDDocumentBuilder *builder,
    int multisig);
```

This method is used to set multi-signature rule.

```c
int DIDDocumentBuilder_RenewSelfProclaimedCredential(
        DIDDocumentBuilder *builder,
        DID *controller,
        DIDURL *signkey,
        const char *storepass);
```

This method mainly uses another controller to re-sign the embedded self-declared Credential. When a Customized DID Document is embedded with a self-declared credential whose signer is the controller, and the controller has been removed, expired, or deactivated, this method needs to be used to repackage the credential.

The controller is designated as the repackaged controller, and theis the signkey key is used by the controller to sign.

```c
int DIDDocumentBuilder_RemoveSelfProclaimedCredential(
        DIDDocumentBuilder *builder,
        DID *controller);
```

This method is used to remove the self-declared credential packaged by a controller.

```c
int DIDDocumentBuilder_RemoveProof(
        DIDDocumentBuilder *builder,
        DID *controller);
```

This method is used to remove the proof signed by a controller. It's generally used when the controller has been removed, expired, or deactivated.
