# Publish DID

上链可以分为有效上链和失效上链。失效上链就是上链是为了告知链上已有的DID已失效，不可再用（该部分内容在后续小节会介绍）；有效上链即是将有效的DID信息更新到链上，该小节介绍该部分的使用说明

The publishing of DID is divided into valid publishing and deactivated publishing. Deactivated publishing is to inform that the DID on the chain has failed and cannot be used again (this part will be introduced in the following section); valid publishing means updating the effective DID information to the chain (relevant instructions will be introduced in this section).

DID Document代表DID上链公开，其提供了Publish DID作为上链的方法。该方法适用于普通DID和非更改持有者信息（Controller和Multisig）的Customized DID有效上链。

The DID Document stands for the publishing of DID, which provides Publish DID as a method of updating DID to the chain. This method is suitable for effectively updating ordinary DID and customized DID without changing the subject’s information (Controller and Multisig) to the chain.

为了防止上链内容被恶意篡改，DID通过自身的Authenication Key对上链内容做签名，接收方根据提供的信息对内容验证签名，以确认上链内容的可靠性。

To prevent the published content from being maliciously tampered with, DID signs the published content with its own authentication key, and the receiver verifies the signature of the content based on the provided information to confirm the reliability of the published content.

## Example

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
let storePass = "pwd";
... ... ... ...
let rootidentity = store.loadRootidentity();
let controller = await identity.newDid(storePass);
//publish controller
await controller.publish(storePass);

// Create customized DID
let did = new DID("did:elastos:helloworld");
//new Customized DID
let doc =  await controller.newCustomized(did, 1, storePass, false);
//publish DID
await doc.publish(storePass);
... ... ... ...
let db = DIDDocument.Builder.newFromDocument(doc).edit();
//add authentication key
let id = DIDURL.from("#test1", db.getSubject());
let key = HDKey.deriveWithPath(HDKey.DERIVE_PATH_PREFIX + 5);
db.addAuthenticationKey(id, doc.getSubject(), key.getPublicKeyBase58());
//seal DID Document
let doc = await db.seal(storePass);
doc.publish(storePass);
... ... ... ...
store.close();
```

## Usage

```typescript
export interface DIDTransactionAdapter {
	createIdTransaction(payload: string, memo: string);
}

public async publish(
	storepass: string,
	inputSignKey: DIDURL | string = null,
	force = false,
	adapter: DIDTransactionAdapter = null
): Promise<void>；
```

inputSignKey是被指定用来签名上链内容的Authentication Key。这里需要说明：普通DID上链只需要任意的Authentication Key皆可；Customized DID的sign key只能是自身的Authentication Key或者是Controller的Default Key。当inputSignKey为null，使用DID Document的Default Key签名上链，普通DID必定有Default Key，但是对于Customized DID只有单签的Customized DID才有Default Key，多签的Customized DID没有。因此多签Customized DID使用默认值null则报错，请务必指定指定Sign Key。

InputSignKey is the authentication key designated to sign the content updated to the chain. Here, it should be noted that the publishing of ordinary DID only requires a random authentication key. However, the sign key of customized DID can only be its own authentication key or the default key of the controller. When the inputSignKey is null, the default key of the DID document is used for signing the published content, and the ordinary DID document must have the default key. However, for customized DIDs, only the customized DID with single signature has the default key, while the multi-signed customized DID does not have one. Therefore, if the multi-signed customized DID uses the default value null, an error will be reported. Please be sure to assign the specified sign key.

force表明在DID Document过期或者本地DID Document未基于最新链上版本进行修改更新是否可以强制上链。一般是建议本地DID Document基于链上最新版本修改，平滑上链。当特殊情况下，设置force为true也可以强制上链。force为true还可以更新已经过期的文档。

Force indicates whether it is possible to force the publishing of data to the chain when the DID document expires or the local DID document is not modified and updated based on the latest version on the chain. Generally, it is recommended that local DID Document be modified based on the latest version on the chain, so as to smoothly publish the document to the chain. Under special circumstances, by setting force to be true, the publishing can also be forced. If force is true, the documents that have expired can also be updated.

adapter为上链提供接口，用户可以自己实现adapter也可以使用Publish DID提供的默认接口。

Adapter provides an interface for publishing the data to the chain. Users can implement the adapter by themselves or use the default interface provided by Publish DID.

注意：对于Customized DID Document无论是更改Controller还是multisig，使用Publish DID方式都无法上链。

Notes: Whether by modifying the controller or multisig, the customized DID document cannot publish the data to the chain by means of Publish DID.
