# Create DIDs from the RootIdentity

## Example

```c
const char *rootPath = "root/store";
DIDStore *store = DIDStore_Open(rootPath);
... ... ... ...
const char *mnemonic = "pact reject sick voyage foster fence warm luggage cabbage any subject carbon";
const char *passphrase = "helloworld";
RootIdentity *identity = RootIdentity_Create(mnemonic, passphrase, true, store, storepass);
if (!identity)
  	//error operation

//create a new DID
DIDDocument *doc = RootIdentity_NewDID(identity, storepass, "", true);
if (!doc)
  	//error operation

DID *did = RootIdentity_GetDIDByIndex(identity, 0);
if (!did)
  	//error operation
  
if (DID_Equals(did, DIDDocument_GetSubject(doc)) != 1)
  	//error operation
  
DID_Destroy(did);
DIDDocument_Destroy(doc);
RootIdentity_Destroy(identity);
... ... ... ...
DIDStore_Close(store);
```

## Usage

```c
DIDDocument *RootIdentity_NewDID(RootIdentity *rootidentity,
        const char *storepass, const char *alias, bool overwrite);
```

该方法提供从RootIdentity衍生成出的新DID，并且将其DID Document保存在DID Store，并返回DID Document。

值得注意的是：该功能不需要提供DID Store，DID Store是由RootIdentity提供（生成RootIdentity是记录下了其相关联的DID Store。）

`overwrite`表示是否需要覆盖已经存在的DID。`overwrite`为true时，覆盖已有DID，返回新生成DID Document object；`overwrite`为false时，若已存在DID，返回NULL。

```c
DIDDocument *RootIdentity_NewDIDByIndex(RootIdentity *rootidentity, int index,
        const char *storepass, const char *alias, bool overwrite);
```

该方法衍生出`rootidentity`的第`index`个DID。其参数和`RootIdentity_NewDID`相同。

```c
void DIDDocument_Destroy(DIDDocument *document);
```

若生成的DID Document不使用后，用该方法来销毁。

```c
DID *RootIdentity_GetDIDByIndex(RootIdentity *rootidentity, int index);
```

该方法用于获取DID对象，过程中不生成也不保存DID Document。主要用于用户需要获取DID对象去执行其他的DID操作。

