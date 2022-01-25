# Create Customized DID

The DID SDK supports customized DID, so there's a corresponding DID Document. There's a cryptographically verifiable hinge between the Subject and the Default Key in primitive DID Document. DID Document finally signs the Document body with this Default Key, which is a closed-loop verification process, and can avoid malicious means such as tampering.

However, the customized DID identification is only an arbitrary string provided by the user, and it's impossible to generate a Key that's cryptographically associated with the DID identification. If you just add an optional Key and sign the DID Document body data with this Key, then you can replace the Key with others’ modified content at any time, which cannot guarantee that DID Document will not be maliciously tampered with. To solve this problem, the Controller and Multisig are introduced. A Controller is the actual controller of Customized DID Document, and Multisig is a multi-signature rule for multiple controllers.

According to the number of controllers, Create Customized DID can be divided into single and multiple signs. This section provides two examples of single signs.

## Example

```c
    DIDStore *store = DIDStore_Open("/DID Store");
    if (!store)
        return;
    ... ... ... ...
    //'littlefish' has only controller1
    DIDDocument *customizedDoc = DIDDocument_NewCustomizedDID(controller1Doc, "littlefish",
                NULL, 0, 0, false, storepass);
    if (!customizedDoc)
      	goto errorExit;

    //edit 'littlefish'
    DIDDocumentBuilder *builder = DIDDocument_Edit(customizedDoc, controller2Doc);
		DIDDocument_Destroy(customizedDoc);
		if (!builder)
      	goto errorExit;

		... ... ... ...
    //add Public Key, Authentication Key, Credential ans service. The sample to add controller is in 'create-customized did'

    //seal builder
    customizedDoc = DIDDocumentBuilder_Seal(builder, storePass);
		DIDDocumentBuilder_Destroy(builder);
		
errorExit:
	DIDStore_Close(store);
  return;
```

## Usage

```c
DIDDocument *DIDDocument_NewCustomizedDID(
        DIDDocument *document,
        const char *customizeddid,
        DID **controllers, size_t size,
        int multisig,
        bool force,
        const char *storepass);
```

This method is used to generate Customized DID Document by Controller Document.

document is the document of a controller; customizedid is a Customized DID string; controllers are arrays of controllers, which may or may not contain the controller that initiated the Customized Document. If controllers are not given, it means that the Customized DID only contains the controller that initiated the Customized Document.&#x20;

size is the number of controllers in the controller array; multisig is a multi-signature rule, which specifies several controllers for signing; force is used to indicate whether to overwrite the Customized Document when it exists locally.

```c
int DIDDocument_IsCustomizedDID(DIDDocument *document);
```

The method can be used to check whether DID Document is Customized DID Document.
