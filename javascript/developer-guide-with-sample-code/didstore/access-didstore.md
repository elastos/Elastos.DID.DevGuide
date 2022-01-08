# Access DIDStore

## Read the Objects in the DID Store

DID Store offers a series of load API for reading objects in the store, which supports the reading of RootIdentity, DIDDocument, and VerifiableCredential.

However, for security reasons, instead of being read directly, the private key stored in the store can only be used transparently through the signature or encrypted API.

* #### Example

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
// Load RootIdentity
let rootidentity = await store.loadRootIdentity();
if (rootidentity) {
    ... ... ...
}
let did = new DID("did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y");
// Load DIDDocuemnt
let doc = await store.loadDid(did);
... ... ... ...
let id = DIDURL.from("#cred-1", did);
// Load VerifiableCredential
let vc = await store.loadCredential(id);
... ... ... ...
store.close();
```

* #### Usage

```typescript
public async loadRootIdentity(
    id: string = undefined
): Promise<RootIdentity>;
```

id is an identifying string of RootIdentity. If this DIDStore has only one RootIdentity, you can use the API without identifying the ID string, such as the example above. Otherwise, the acquisition will fail.

RootIdentity object is the obtained result.

```typescript
public async loadDid(
    didOrString: DID | string
): Promise<DIDDocument>;
```

The parameter can be the DID object or the identifying string of DID, the name of which is consistent with the file name saved in DIDStore. If the DID content does not exist or the DID document is saved in the wrong format, the acquisition will fail.&#x20;

**Disclaimer:** There are many ways to get the DID object - the way shown in the example is just one of them, and users can use API according to their own needs. See DID/Create DID and DIDDocument for details.

DID document object is the obtained result, which is also the DID content saved in the DIDStore.

```typescript
public async loadCredential(
    id: DIDURL | string
): Promise<VerifiableCredential>；
```

id is the unique identifier of the credential, so the credential object should be obtained through it. id can be a DIDURL object or an ID character string. If the credential does not exist or the format is incorrect, the acquisition will fail.&#x20;

**Disclaimer:** There are many ways to get the DID object **-** the way shown in the example is just one of them, and users can use API according to their own needs. See DID/Create DID and DIDDocument for details.

Credential object is the obtained result.

## Save DID Information in the DID Store

DID store provides a series of APIs, mainly including DID document, verifiable credential and private key, for saving DID information to the DID store.

Among them, saving the private key is mainly used when DIDDocument adds the key, and the public key in the key pair is added to the document, while the private key is encrypted and saved to the DID store for authorization and entrustment of DID.

* #### Example

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
... ... ... ...
let did = new DID("did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y");
let doc = await did.resolve();
if (doc)
    // store DIDDocument
    await store.storeDid(doc);
... ... ... ...
let storePass = "pwd";
let id = DIDURL.from("#key-1", did);
let sk = HDKey.deserializeBase58("xprv9s21ZrQH143K4biiQbUq8369meTb1R8KnstYFAKtfwk3vF8uvFd1EC2s49bMQsbdbmdJxUWRkuC48CXPutFfynYFVGnoeq8LJZhfd9QjvUt").serialize();
//store PrivateKey
store.storePrivateKey(id, sk, storePass);
... ... ... ...
let issuer = new Issuer(doc);
let vc = issuer.issueFor(did)
                    .id("#test")
                    .type("SelfProclaimedCredential")
                    .properties({"name": "littlefish"})
                    .seal(storePass);
if (vc)
    //store Credential
    await store.storeCredential(vc);
... ... ... ...
store.close();
```

* #### Usage

```typescript
public async storeDid(
    doc: DIDDocument
): Promise<void>;
```

The SDK saves the DID content in the DID store.

Parameter doc: There are many ways to obtain DIDDocument, and the way shown in the example is only one of them.

```typescript
public async storeCredential(
    credential: VerifiableCredential
): Promise<void>;
```

SDK saves the credential object in the DID store.

```typescript
public storePrivateKey(
    idOrString: DIDURL | string,
    privateKey: Buffer,
    storepass: string
):void;
```

Parameter privateKey: 82-bit extended private key

Parameter storepass: for the encryption of private keys

## List the Objects in DID Store

DID store refers to background storage. By enumerating, users can get to know all DID information stored in DID Store. DID Store offers ways of enumerating RootIdentity, DIDs, and verifiable credentials.

For security reasons, the private key cannot be listed.

* #### Example

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
... ... ... ...
//list RootIdentities
let ids = await store.listRootIdentities();
if (ids.length > 0) 
    console.log("there are {} root identities in the DID store.", ids.length);
else
    console.log("there are no root identity in the DID store.");
... ... ... ...   
//list DIDs
let remains = await store.listDids();
if (remains.length > 0) {
    console.log("there are {} dids in the DID store.", remains.length);
    //list Credentials
    let vcs = await store.listCredentials(remains[0]);
    if (vcs.length > 0)
        console.log("did: {} has {} credentials.", remains[0], vcs.length);
    else
        console.log("did: {} has no credential.", remains[0]); 
} else {
    console.log("there are no did in the DID store");
}
... ... ... ...
store.close();
```

* #### Usage

```typescript
public async listRootIdentities(): Promise<RootIdentity[]>;
```

List the RootIdentity saved in the DID Store and return the RootIdentity object in an array.

```typescript
public async listDids(): Promise<DID[]>;
```

List the DID saved in the DID Store and return the DID in an array. To gain more detailed DID content, users can obtain the DID document by loading DID.

```typescript
public async listCredentials(didOrString: DID | string): Promise<DIDURL[]>;
```

List all the credentials of the specified DID and return the ID in an array. A  more detailed credential object can be obtained by loading credential.

## Select the Objects in the DID Store

With list function, all objects of the same type in the DID store can be listed, and the select function allows users to list DID and verifiable credential objects that match the specified conditions.

* #### Example

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
... ... ... ...
//select DIDs
await store.selectDids(new class implements DIDFilter {
    public select(d: DID): boolean {
        return d.resolve() ? true : false;     
    }
});
... ... ... ... 
//select credentials
await store.selectCredentials(did, new class implements DIDFilter {
    public select(id: DIDURL): boolean {
        return true;     
    }
});
... ... ... ...
store.close();
```

* #### Usage

```typescript
export interface DIDFilter {
    select(did: DID): boolean;
}

public async selectDids(filter: DIDStore.DIDFilter): Promise<DID[]>;
```

Users implement the DIDFilter to provide a method for selecting DID. See “Sample” for concrete usage.

```typescript
export interface CredentialFilter {
    select(id: DIDURL): boolean;
}
public async selectCredentials(
    didOrString: DID | string,
    filter: DIDStore.CredentialFilter
): Promise<DIDURL[]>;
```

Users implement CredentialFilter to provide the method of selecting the credentials and returning the qualified Credential Id identifier.

## Delete the Objects in the DID Store

The DID Store provides a method of saving DID objects, so it esentially offers a way of deleting the DID objects that have been saved. RootIdentity, DID, verifiable credential and private key can be deleted first.

* #### Example

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
... ... ... ...
let ids = await store.listRootIdentities();
if (ids.length > 0) {
    //delete RootIdentity
    if (store.deleteRootIdentity(ids[0].getId()))
        console.log("delete root identity {} sucessfully.", ids[0].getId());
    else
        console.log("delete root identity {} failed.", ids[0].getId());
}      
... ... ... ...
let dids = await store.listDids();
if (dids.length > 0) {
    console.log("there are {} dids in the DID store.", dids.length);
    let vcs = await store.listCredentials(dids[0]);
    if (vcs.length > 0) {
        console.log("did: {} has {} credentials.", dids[0], vcs.length);
        //list Verifiable Credential
        if (store.deleteCredential(vcs[0]))
            console.log("delete credential {} successfully.", vcs[0]);
        else
            console.log("delete credential {} failed.", vcs[0]);
    } else {
        console.log("did: {} has no credential.", dids[0]); 
    }

    //delete DID
    if (store.deleteDid(dids[0]))
        console.log("delete did {} successfully.", dids[0]);
    else
        console.log("delete did {} failed.", dids[0]);
} else {
    console.log("there are no did in the DID store");
}
... ... ... ...
store.close();
```

* #### Usage

```typescript
public deleteRootIdentity(id: string): boolean;
```

Delete RootIdentity according to the ID string identifier. If there is no such RootIdentity, or the RootIdentity is the only object in DID store and cannot be deleted, then return false.

```typescript
public deleteDid(didOrString: DID | string): boolean;
```

According to the DID identifier, delete all content related to didOrString stored in DIDStore, e.g. documents, credentials and private keys. If there is no such DID information or an error occurs, then return false.

```typescript
public deleteCredential(idOrString: DIDURL | string): boolean;
```

Delete the credential according to the ID identifier. If there is no such DID information or an error occurs, then return false.

```typescript
public deletePrivateKey(idOrString: DIDURL | string): boolean;
```

Delete the corresponding private key according to the ID identifier. If there is no information about such private key or an error occurs, then return false.

## Check whether there are Objects in the DID Store

Sometimes users only need to know whether there is any object stored in the DID Store without having to provide any instance of this object. Therefore, DID Store provides methods to check the existence of RootIdentity, Mnemonic, DID, Verifiable Credential, and Private key.

* #### Example

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
//contains RootIdentities
if (store.containsRootIdentities()) {
    console.log("there are root identities in the DID store.");
    let ids = await store.listRootIdentities();
    let id = ids[0].getId();
    //contains RootIdentity
    if (store.containsRootIdentity(id)) {
        console.log("root identity {} is in the DID store.", id);
        //contains RootIdentity Mnemonic
        if (store.containsRootIdentityMnemonic(id)
             console.log("root identity {} has mnemonic.", id);
         else
             console.log("root identity {} doesn't have mnemonic.", id);
    } else {
        console.log("root identity {} isn't in the DID store.", id);
    }
} else {
    console.log("there are no root identity in the DID store.");
}
//contains DIDs
if (store.containsDids()) {
    console.log("there are DIDs in the DID store.");
    let dids = await store.listDIDs();
    let did = dids[0];
    //contains DID
    if (store.containsDid(id)) 
        console.log("DID {} is in the DID store.", did);
    else
        console.log("DID {} isn't in the DID store.", did);
} else {
    console.log("there are no DID in the DID store.");
}
//contains Credentials
if (store.containsCredentials(did)) {
    console.log("there are some credentials in the DID store.");
    let vcs = await store.listCredentials();
    let vc = vcs[0];
    //contains Credential
    if (store.containsCredential(vc))
        console.log("Credential {} is in the DID store.", vc);
    else
        console.log("Credential {} isn't in the DID store.", vc);
} else {
    console.log("there are no credential in the DID store.");
}
... ... ... ...
store.close();
```

* #### Usage

```typescript
public containsRootIdentity(id: string): boolean;
```

id is the identifying string of RootIdentity. If there is no such RootIdentity in the DID store or any other error occurs, then return false.

```typescript
public containsRootIdentities(): boolean;
```

Check if there is any RootIdentity in the DID store. If the DID store contains any RootIdentity, then return true; otherwise, return false.

```typescript
public containsRootIdentityMnemonic(id: string): boolean;
```

Check if there is a mnemonic in the specified RootIdentity.

```typescript
public containsDid(did: DID | string): boolean;
```

Check if there is a specified DID in the DID store.

```typescript
public containsDids(): boolean;
```

Check if there is a DID in the DID Store. If any DID is found in the DID Store, then return true; otherwise, return false.

```typescript
public async containsCredential(id: DIDURL | string): Promise<boolean>;
```

Check if there is the credential of the specified ID in the DID Store.

```typescript
public containsCredentials(did : DID) : boolean;
```

Check if there is the credential of the specified DID in the DID Store. If any credential is found, then return true; otherwise, return false.

```typescript
public containsPrivateKey(id: DIDURL | string): boolean;
```

Check if there is the private key of the specified id in the DID Store.

```typescript
public containsPrivateKeys(did: DID | string): boolean;
```

Check if there is the private key of the specified DID in the DID Store. If any private key if found, then return true; otherwise, return false.
