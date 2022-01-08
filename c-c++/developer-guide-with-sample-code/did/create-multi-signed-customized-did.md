# Create multi-signed customized DID

本小节主要是介绍多签的Customized DID Document的生成方法和修改Customized DID Document中普通Document没有的方法。

This section mainly introduces the generation method of multi-signed Customized DID Document and the method of modifying the Customized DID Document which is not available in primitive Document.

多签（m:n）规则，只要n>1即为多签。m表示签名者个数，即Document 的Proof中signature个数。如果signature个数小于m则表明此时的Document还不是真正可用有效的多签文档，需要剩余未签名的Controller完成签名工作。

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

普通DID Document的那些添加和移除元素的方法同样适合Customized DID Document，这里说明的是Customized DID Document特有的内容。

The methods of adding and removing elements of primitive DID Document are also suitable for Customized DID Document. Here, the unique content of Customized DID Document is described.

```c
DIDDocument *DIDDocument_SignDIDDocument(
        DIDDocument* controllerdoc,
        const char *document,
        const char *storepass);
```

该方法用于多签，当Customized Document的签名未达到多签规则，则需要其他controller使用该方法来对文档签名。

This method is used for multi-signature. When the signature of Customized Document does not meet the multi-signature rule, other controllers need to use this method to sign the document.

当第一个controller通过`DIDDocument_NewCustomizedDID`方法得到文档， DID Document object可以通过`DIDDocument_IsQualified`来检查文档是否符合多签规则，若还未达到，则其他controller则使用该方法继续对文档签名。

When the first controller gets the document by DIDDocument\_NewCustomizedDID method, DID Document object can check whether the document meets the multi-signature rule by DIDDocument\_IsQualified, and if not, other controllers use this method to continue to sign the document.

`document`需要继续签名的文档数据。

Document data which is continued to be signed is needed by the _document_.

```c
const char *DIDDocument_MergeDIDDocuments(int count, ...);
```

该方法用于merge多个文档（可以是符合多签规则文档也可以是未符合多签规则文档），去掉count文档里相同的controller的签名，合并不同签名，以统一文档。

This method is used to merge multiple documents (whether the documents meet the multi-signature rule or not), remove the signatures of the same controller in the count document, and merge different signatures to unify the documents.

举例：controller1发起创建Customized Document，多签规则3:4，生成doc1；controller2通过`DIDDocument_SignDIDDocument`对doc1再添加签名得到doc2，controller3同时通过`DIDDocument_SignDIDDocument`对doc1再添加签名得到doc3，此时完成了多签规则中三个controller的签名，但是doc2和doc3都不符合多签规则。User就可以通过`DIDDocument_MergeDIDDocuments`来得到controller1，controller2，controller3签名的完整文档。

Example: controller1 initiates the creation of Customized Document, multi-signature rule 3:4, and generate doc1; Controller2 adds another signature to doc1 by DIDDocument\_SignDIDDocument to get doc2, and controller3 adds another signature to doc1 by DIDDocument\_SignDIDDocument to get doc3. At this time, the signatures of three controllers in the multi-signature rule are completed, but neither doc2 nor doc3 meets the multi-signature rule. User can get the complete documents signed by controller1, controller2 and controller3 through DIDDocument\_MergeDIDDocuments.

```c
int DIDDocument_IsQualified(DIDDocument *document);
```

该方法告知当前的Customized Document中signature个数是否符合多签规则。

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

上面两个方法很明显的是Customized DID Document才有的方法，添加和移除Controller，用于普通DID Document，则报错。

The above two methods obviously belong to Customized DID Document. If the actions of adding and removing Controller are done to the primitive DID Document, an error will be reported.

这里需要说明的是：假如DID Document若有需要删除的Controller签名的Credential，无法直接删除。需要renew或者remove这个凭证。若只剩最后一个controller，也会失败。

It should be noted here that if DID Document has the Credential signed by the Controller that needs to be deleted, it cannot be deleted directly. The credential needs to be renewed or removed. If there is only the last controller left, it will also fail.

```c
int DIDDocumentBuilder_SetMultisig(
    DIDDocumentBuilder *builder,
    int multisig);
```

该方法用来设置多签规则。

This method is used to set multi-signature rule.

```c
int DIDDocumentBuilder_RenewSelfProclaimedCredential(
        DIDDocumentBuilder *builder,
        DID *controller,
        DIDURL *signkey,
        const char *storepass);
```

该方法主要换一个controller对内嵌的自声明Credential重新签名。当Customized DID Document内嵌某自声明凭证，其签名者为controller，而该controller已经被移除，过期或者被Deactivated，则需要使用该方法对该凭证重新封装。

This method mainly uses another controller to re-sign the embedded self-declared Credential. When a Customized DID Document is embedded with a self-declared credential whose signer is the controller, and the controller has been removed, expired, or deactivated, this method needs to be used to repackage the credential.

`controller`指定为重新封装的controller，`signkey`为controller用来签名的sign key。

The _controller_ is designated as the repackaged controller, and the _signkey_ is the sign key used by the controller to sign.

```c
int DIDDocumentBuilder_RemoveSelfProclaimedCredential(
        DIDDocumentBuilder *builder,
        DID *controller);
```

该方法用来移除某个controller封装的自声明凭证。

This method is used to remove the self-declared credential packaged by a controller.

```c
int DIDDocumentBuilder_RemoveProof(
        DIDDocumentBuilder *builder,
        DID *controller);
```

该方法用来移除某个controller签名的的proof，一般用于该controller已经被移除，过期或者失效的情况下。

This method is used to remove the proof signed by a controller. It is generally used when the controller has been removed, expired, or deactivated.
