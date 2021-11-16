# Access DIDStore

## 读取DID store中的对象

DID Store提供了一系列的load API用于DID Store内的对象读取，可以支持读取DID Store中的RootIdentity、DIDDocument、VerifiableCredential。

但是处于安全的考虑，存储在DID Store中的私钥是不能被直接读取的，只能通过签名或者加密的API透明的使用。

* #### Example

```c
const char *rootPath = "root/store";
DIDStore *store = DIDStore_Open(rootPath);

// Load RootIdentity
RootIdentity *rootidentity = DIDStore_LoadDefaultRootIdentity(store);
if (rootidentity) {
    ... ... ...
    RootIdentity_Destroy(rootidentity);
}

DID *did = new DID("did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y");
// Load DIDDocuemnt
DIDDocument *doc = DIDStore_LoadDid(store, did);
... ... ... ...
DID_Destroy(did);
DIDDocument_Destroy(doc);

DIDURL *id = DIDURL_NewFromDid(did, "cred-1");
// Load VerifiableCredential
Credential *vc = DIDStore_LoadCredential(store, DIDURL_GetDid(id), id);
... ... ... ...
DIDURL_Destroy(id);
Credential_Destroy(vc);
... ... ... ...
DIDStore_Close(store);
```

* #### Usage

```c
RootIdentity *DIDStore_LoadRootIdentity(DIDStore *store, const char *id);
```

`id`是RootIdentity的标识字符串。如果该DID Store就一个RootIdentity，那么在使用该api时可以不给`id`标识，如上面的example。否则，获取失败。

获取结果是RootIdentity object。注意：用完RootIdentity object，用`RootIdentity_Destroy`销毁。

```c
DIDDocument *DIDStore_LoadDID(DIDStore *store, DID *did);
```

如果该`did`内容不存在或者DID Document保存格式有误，则获取失败。声明：有多种方式可以获取DID object，example里只是其中一种，用户根据自己需求来使用API。具体内容详见DID/Create DID and DIDDocument章节。

获取结果是DID Document object（也是DID Store保存的DID内容）。注意：用完DID Document object，用`DIDDocument_Destroy`销毁。

```c
Credential *DIDStore_LoadCredential(
    DIDStore *store,
    DID *did,
    DIDURL *credid);
```

`did`是Credential所属的DID；`credid`是Credential的唯一标识符。如果该凭证不存在或者格式有误，则获取失败。声明：有多种方式可以获取DIDURL object，example里只是其中一种，用户根据自己需求来使用API。具体内容详见DID/Create DID and DIDDocument章节。

获取结果是Credential object。注意：用完Credential object，用`Credential_Destroy`销毁。

## 保存DID信息到DID store

DID Store提供一系列API用于保存DID信息到DID Store，主要有DID Document，Verifiable credential和Private key。

其中，保存私钥主要用于DIDDocument添加key时，密钥对中的公钥添加到Document，私钥加密保存到DID Store，用于DID的授权和委托。

* #### Example

```c
const char *rootPath = "root/store";
DIDStore *store = DIDStore_Open(rootPath);
... ... ... ...
DID *did = new DID("did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y");
int status = 0;
DIDDocument *doc = DID_Resolve(did, &status, true);
if (doc) {
    // store DIDDocument
    DIDStore_StoreDid(store, doc);
  	DIDDocument_Destroy(doc);
}
... ... ... ...
const char *storePass = "pwd";
DIDURL *id = DIDURL_NewFromDid(did, "key-1");
//store PrivateKey
if (DIDStore_StorePrivateKey(store, id, sk, storePass) < 0)
  	//error operation
DIDURL_Destroy(id);

... ... ... ...
DIDURL *credid = DIDURL_NewFromDid(did, "mycredential");
Issuer *issuer = Issuer_Create(did, NULL, store);
const char *types[] = {"BasicProfileCredential", "PhoneCredential",
                       "SelfProclaimedCredential"};
Property props[2];
props[0].key = "name";
props[0].value = "John";
props[1].key = "gender";
props[1].value = "Male";

Credential *vc = Issuer_CreateCredential(issuer, did, credid, types, 3, props, 2,
        expires, storePass);
Issuer_Destroy(issuer);
DIDURL_Destroy(credid);
if (vc) {
    //store Credential
    DIDStore_StoreCredential(store, vc);
  	Credential_Destroy(vc);
}
DID_Destroy(did);
... ... ... ...
DIDStore_Close(store);
```

* #### Usage

```c
int DIDStore_StoreDID(DIDStore *store, DIDDocument *document);
```

该方法将DID Document保存在DID Store。

```c
int DIDStore_StoreCredential(DIDStore *store, Credential *credential);
```

该方法将Credential object保存在DID Store。

```c
int DIDStore_StorePrivateKey(
        DIDStore *store,
        const char *storepass,
        DIDURL *id,
        const uint8_t *privatekey,
        size_t size);
```

`storepass`用于私钥在DID Store中的加密保存；`privateKey`是82位扩展私钥；`size`为私钥的长度。

## 列举DID store中的对象

DID Store是后台存储，用户需要知道DID Store中保存的所有DID信息就可以通过列举方式来获取。DID Store提供RootIdentity，DID和Verifiable Credential的例举方法。

为了安全起见，私钥不可列举。

* #### Example

```c
int get_rootidentity(RootIdentity *rootidentity, void *context)
{
    int *count = (int*)context;

    if (!rootidentity)
        return 0;

    (*count)++;
    return 0;
}

int get_did(DID *did, void *context)
{
    DID *d = (DID*)context;

    if (!did)
        return 0;

    (*count)++;
    return 0;
}

const char *rootPath = "root/store";
DIDStore *store = DIDStore_Open(rootPath);
... ... ... ...

//list RootIdentities
int count = 0;
if (DIDStore_ListRootIdentities(store, get_rootidentity, (void*)&count) < 0)
  //error operation
  
if (count > 0) 
    printf("there are %d root identities in the DID store.\n", count);
else
    printf("there are no root identity in the DID store.\n");
... ... ... ...   

//list DIDs
count = 0;
if (DIDStore_ListDIDs(store, 0, get_did, (void*)&count) < 0)
  //error operation
  
if (count > 0)
    printf("there are %d dids in the DID store.\n", count);
... ... ... ...

DIDStore_Close(store);
```

* #### Usage

```c
typedef int DIDStore_RootIdentitiesCallback(RootIdentity *rootidentity, void *context);

ssize_t DIDStore_ListRootIdentities(
        DIDStore *store,
        DIDStore_RootIdentitiesCallback *callback,
        void *context);
```

列举DID Store中保存的RootIdentity，并将RootIdentity object传递给`callback`中传递给用户。

```c
typedef int DIDStore_DIDsCallback(DID *did, void *context);
typedef enum {
    DIDFilter_All = 0,
    DIDFilter_HasPrivateKey = 1,
    DIDFilter_WithoutPrivateKey = 2
} ELA_DID_FILTER;

int DIDStore_ListDIDs(
        DIDStore *store,
        ELA_DID_FILTER filer,
        DIDStore_DIDsCallback *callback,
        void *context);
```

列举DID Store中保存的DID，并将DID传递给`callback`中传递给用户。`filer`是DID过滤器，用来表明选择DID的条件。

```c
typedef int DIDStore_CredentialsCallback(DIDURL *id, void *context);

int DIDStore_ListCredentials(
        DIDStore *store,
        DID *did,
        DIDStore_CredentialsCallback *callback,
        void *context);
```

列举指定DID的所有凭证，并将凭证ID传递给`callback`中传递给用户。

## 挑选DID Store中的对象

List功能可以列举DID Store中所有同类型对象，select功能可以列举符合指定条件的DID和Verifiable Credential对象。

* #### Example

```c
const char *rootPath = "root/store";
DIDStore *store = DIDStore_Open(rootPath);
... ... ... ...
Credential *vcs[1];
//select Credentials
if (DIDDocument_SelectCredentials(customized_doc, "SelfProclaimedCredential",
            NULL, vcs, sizeof(vcs) < 0)
 		//error operation
... ... ... ...
DIDStore_Close(store);
```

* #### Usage

```c
ssize_t DIDDocument_SelectCredentials(
        DIDDocument *document,
        const char *type,
        DIDURL *credid,
        Credential **creds,
        size_t size);
```

该方法根据`type`和`credid`来选择符合条件的Credential的方法，返回符合要求的Credential Id放在`creds`数组里。

`creds`是用户提供的用来存放符合条件的Credential的Id的数组，`size`为数组大小。

`type`和`credid`至少有一个有值，否则报错。

## 删除DID Store中的对象

DID Store提供方法保存DID对象，自然也提供删除保存的DID对象的方法，先课删除RootIdentity，DID，Verifiable Credential和Private key。

* #### Usage

```c
bool DIDStore_DeleteRootIdentity(DIDStore *store, const char *id);
```

该方法用来删除标识为`id`的RootIdentity。若无该RootIdentity，或者该RootIdentity是DID store中唯一的对象，不可删除，则返回false。

```c
bool DIDStore_DeleteDID(DIDStore *store, DID *did);
```

该方法用来删除保存在DID Store内所有和`did`有关的内容，如文档，凭证和私钥。若无该DID信息或者发生错误，则返回false。

```c
bool DIDStore_DeleteCredential(DIDStore *store, DID *did, DIDURL *id);
```

该方法用来删除Credential。若无该Credential信息或者发生错误，则返回false。

`did`是Credential的所有者，`id`为Credential标识。

```c
void DIDStore_DeletePrivateKey(DIDStore *store, DIDURL *keyid);
```

该方法提供删除`keyid`的私钥。若无该私钥信息或者发生错误，则返回false。

## 检查DID Store中是否包含对象

有些时候用户只需要知道DID store中是否保存某对象，而不需要该对象实例。因此DID store提供了方法检查RootIdentity，Mnemonic，DID，Verifiable Credential和Private key的存在与否。

* #### Usage

```c
int DIDStore_ContainsRootIdentity(DIDStore *store, const char *id);
```

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, didstore doestn't contain rootidentiy;
  ```
* ```
   return value = 1, didstore contains rootidentiy.
  ```

```c
int DIDStore_ContainsRootIdentities(DIDStore *store);
```

检查DID Store是否含有RootIdentity。

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, there isn't rootidentity in didstore;
  ```
* ```
   return value = 1, there is rootidentity in didstore.
  ```

```c
int DIDStore_ContainsRootIdentityMnemonic(DIDStore *store, const char *id);
```

检查指定RootIdentity是否含有mnemonic。

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, there isn't rootidentity's mnemonic in didstore;
  ```
* ```
   return value = 1, there is rootidentity's mnemonic in didstore.
  ```

```c
int DIDStore_ContainsDID(DIDStore *store, DID *did);
```

检查DID Store是否含有指定DID。

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, did isn't in didstore;
  ```
* ```
   return value = 1, did is in didstore.
  ```

```c
int DIDStore_ContainsDIDs(DIDStore *store);
```

检查DID Store是否含有DID。

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, no did isn't in didstore;
  ```
* ```
   return value = 1, did is in didstore.
  ```

```c
int DIDStore_ContainsCredential(DIDStore *store, DID *did, DIDURL *credid);
```

检查DID store是否含有指定id的Credential。

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, credential isn't in didstore;
  ```
* ```
   return value = 1, credential is in didstore.
  ```

```c
int DIDStore_ContainsCredentials(DIDStore *store, DID *did);
```

检查DID store是否含有指定DID的Credential。

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, there isn't credential in didstore;
  ```
* ```
   return value = 1, there is credential in didstore.
  ```

```c
int DIDStore_ContainsPrivateKey(DIDStore *store, DID *did, DIDURL *keyid);
```

检查DID store是否含有指定id的Private key。

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, there isn't private key in didstore;
  ```
* ```
   return value = 1, there is private key in didstore.
  ```

```c
int DIDSotre_ContainsPrivateKeys(DIDStore *store, DID *did);
```

检查DID store是否含有指定did的私钥。

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, there isn't private key in didstore;
  ```
* ```
   return value = 1, did is deacativated.
  ```
