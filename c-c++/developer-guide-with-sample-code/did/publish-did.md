# Publish DID

## Publish DID

上链可以分为有效上链和失效上链。失效上链就是上链是为了告知链上已有的DID已失效，不可再用（该部分内容在后续小节会介绍）；有效上链即是将有效的DID信息更新到链上，该小节介绍该部分的使用说明

DID Document代表DID上链公开，其提供了Publish DID作为上链的方法。该方法适用于普通DID和非更改持有者信息（Controller和Multisig）的Customized DID有效上链。

为了防止上链内容被恶意篡改，DID通过自身的Authenication Key对上链内容做签名，接收方根据提供的信息对内容验证签名，以确认上链内容的可靠性。

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

`signkey`是被指定用来签名上链内容的Authentication Key。这里需要说明：普通DID上链只需要任意的Authentication Key皆可；Customized DID的sign key只能是自身的Authentication Key或者是Controller的Default Key。当`signkey`为NULL，使用DID Document的Default Key签名上链，普通DID必定有Default Key，但是对于Customized DID只有单签的Customized DID才有Default Key，多签的Customized DID没有。因此多签Customized DID使用默认值NULL则报错，请务必指定Sign Key。

_signkey_ is the Authentication Key designated to sign the cochain content. Here, it should be noted that any Authentication Key can be used to the primitive DID cochain; The sign key of Customized DID can only be its own Authentication Key or the Default Key of the Controller. When signkey is NULL, the Default Key signature of DID Document is used for cochain. Primitive DID must have a Default Key, but for Customized DID, only the Customized DID with single signature has a Default Key, and the Customized DID with multiple signatures does not. Therefore, if multi-signed Customized DID uses the default value NULL, an error will be reported. Please be sure to specify Sign Key.

`force`表明在DID Document过期或者本地DID Document未基于最新链上版本进行修改更新是否可以强制上链。一般是建议本地DID Document基于链上最新版本修改，平滑上链。当特殊情况下，设置`force`为true也可以强制上链。`force`为true还可以更新已经过期的文档。

_force_ indicates whether the action of cochain can be done forcedly when DID Document expires or the local DID Document is not modified and updated based on the latest version on the chain. Generally, it is recommended that local DID Document be modified based on the latest version on the chain, so as to be on chain smoothly. Under special circumstances, setting force to true can also complete the action of cochain forcedly. When _force_ is true, the documents expired can be updated.

注意：对于Customized DID Document无论是更改Controller还是multisig，使用Publish DID方式都无法上链。

Attention: For Customized DID Document, whether it is to change the Controller or multisig, the action of cochain cannot be completed by Publish DID.
