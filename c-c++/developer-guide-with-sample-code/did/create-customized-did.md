# Create customized DID

DIDSDK 支持自定义DID，那么也就需要有对应的DID Document。普通DID Document里的Subject和Default Key之间存在密码学可验证的关键，DID Document最后用这个Default Key来签名Document主体是有个闭环的验证过程，可以避免被篡改等恶意手段。

DID SDK supports customized DID, so there is a corresponding DID Document. There is a cryptographically verifiable hinge between the Subject and the Default Key in primitive DID Document. DID Document finally signs the Document body with this Default Key, which is a closed-loop verification process, and can avoid malicious means such as tampering.

但是自定义DID标识只是用户提供的任意字符串，没法生成和DID标识有密码学相关联的Key。如果只是添加随意的Key，用该Key签名DID Document主体数据，那么随时都可以被他人修改内容替换Key，无法保证DID Document的不被恶意篡改。为了解决这个问题，引入了Controller和Multisig。Controller即为Customized DID Document的实际持有者，Multisig是多个Controller的多签规则。

However, the customized DID identification is only an arbitrary string provided by the user, and it is impossible to generate a Key which is cryptographically associated with the DID identification. If you just add an optional Key and sign the DID Document body data with this Key, then you can replace the Key with others’ modified content at any time, which cannot guarantee that DID Document will not be maliciously tampered with. To solve this problem, the Controller and Multisig are introduced. A Controller is the actual controller of Customized DID Document, and Multisig is a multi-signature rule for multiple controllers.

根据controller的个数，Create Customized DID可以分为单签和多签两个情况，本小节提供两种单签的示例。

According to the number of controllers, Create Customized DID can be divided into single sign and multiple signs. This section provides two examples of single sign.

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

Controller Document用该方法生成Customized DID Document。

This method is used to generate Customized DID Document by Controller Document.

`document`是某个controller的document；`customizeddid`是Customized DID字符串；`controllers`是controller的数组，可含该发起Customized Document的controller，也可不含，若不给controllers，表明该Customized DID只含有发起Customized Document的这一个controller；`size`是controller数组中的controller数量；`multisig`是多签规则，指定几个controller用来签名；`force`用来表明当本地有该Customized Document时是否覆盖。

_document_ is the document of a controller; _customizeddid_ is a Customized DID string; _controllers_ are arrays of controllers, which may or may not contain the controller that initiated the Customized Document. If controllers are not given, it means that the Customized DID only contains the controller that initiated the Customized Document. _size_ is the number of controllers in the controller array; _multisig_ is a multi-signature rule, which specifies several controllers for signing; _force_ is used to indicate whether to overwrite the Customized Document when it exists locally.

```c
int DIDDocument_IsCustomizedDID(DIDDocument *document);
```

该方法可以用来检查DID Document是否为Customized DID Document。

The method can be used to check whether DID Document is Customized DID Document.
