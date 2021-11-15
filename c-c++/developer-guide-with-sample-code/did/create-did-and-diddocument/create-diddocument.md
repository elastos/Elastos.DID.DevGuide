# Create DIDDocument

DID Document 分为普通DID Document和自定义DID Document，这里讲的内容都是针对普通DID Document。自定义DID Document将在后续章节详细介绍。

通过newDid来生成DID Document，该DID Document是只含有default key的最基础DID Document。用户可以用过DID Document Builder来修改DID Document 内容，完成修改，通过seal方法终止修改，得到新DID Document。

DID Document 有public key，authentication key，authorization key，verifiable credential和service五种元素，通过相应的添加或者移除方法来修改文档。具体的方法请参考API doc。

## Example

```c
//get did document builder
DID *did = DIDDocument_GetSubject(doc);
DIDDocumentBuilder *builder = DIDDocument_Edit(doc, NULL);

// Add 2 public keys
DIDURL *id1 = DIDURL_NewFromDid(did, "test1");
... ... ... ... 
if (DIDDocumentBuilder_AddPublicKey(builder, id1, did, keybase) == 0)
  printf("add 'test1' key successfully.");

if（DIDDocumentBuilder_AddAuthenticationKey(builder, id1, NULL) == 0)
  printf("add 'test1' key to authentication key successfully.");

... ... ... ...
DIDDocument *newDoc = DIDDocumentBuilder_Seal(builder, storePass);
DIDDocumentBuilder_Destroy(builder);
DIDURL_Destroy(id1);
... ... ... ...
```

## Usage

newDid方法在RootIdentity章节介绍，这里不再多加说明了。

```c
DIDDocumentBuilder* DIDDocument_Edit(DIDDocument *document, DIDDocument *controllerdoc);
```

该方法获取DIDDocumentBuilder object用于更改文档。

`controllerdoc`参数是针对自定义 DID Document修改，指定用于第一次签名的controller，对于普通DID Document，无须给`controllerdoc`。

```c
void DIDDocumentBuilder_Destroy(DIDDocumentBuilder *builder);
```

该方法提供销毁DIDDocumentBuilder object的方法。

```c
DIDDocument *DIDDocumentBuilder_Seal(DIDDocumentBuilder *builder, const char *storepass);
```

该方法封装且得到新的DID Document。其中storepass为DID Store密码，用于对修改后DID Document主体内容通过主key私钥进行签名。

```c
int DIDDocumentBuilder_AddPublicKey(DIDDocumentBuilder *builder,
        DIDURL *keyid, DID *controller, const char *key);
```

该方法添加public key，若`keyid`或者`key`存在则报错。

```c
int DIDDocumentBuilder_RemovePublicKey(DIDDocumentBuilder *builder,
        DIDURL *keyid, bool force);
```

该方法需要注意：default key无法被移除；当希望被移除的Key是Authentication key或者Authorization key时，则需要根据force参数决定是否移除，若force为true，支持移除，否则报错。

```c
int DIDDocumentBuilder_AddAuthenticationKey(DIDDocumentBuilder *builder,
        DIDURL *keyid, const char *key);
```

该方法添加Authentication Key，若Key Id不存在，则生成新Authentication Key；若该Key Id已经存在，`key`和已经存在的Key内容相同，或者该Key Id已经是Authentication Key或者Authorization Key则将该不相同，则报错。

若想添加已知存在的Key，则只需要提供`keyid`, 不需要提供`key`。`key`为公钥的base58字符串。

```c
int DIDDocumentBuilder_RemoveAuthenticationKey(DIDDocumentBuilder *builder,
        DIDURL *keyid);
```

该方法移除已经存在的authentication key，若不存在，则报错。

```c
int DIDDocumentBuilder_AddAuthorizationKey(DIDDocumentBuilder *builder,
        DIDURL *keyid, DID *controller, const char *key);
```

该方法根据提供的Key内容添加Authorization Key，只适用于普通DID Document。若Key Id不存在，则生成新Authorization Key；若该Key Id已经存在，`key`和已经存在的Key内容相同，或者该Key Id已经是Authentication Key或者Authorization Key则将该不相同，则报错。

```c
int DIDDocumentBuilder_AuthorizeDid(DIDDocumentBuilder *builder,
        DIDURL *keyid, DID *controller, DIDURL *authorkeyid);
```

该方法根据提供的controller和controller的Key Id添加新Authorization Key，若已经存在，则报错。

This method is to add the specified key to be an Authorization key. This specified key is the key of specified controller. Authentication is the mechanism by which the controller(s) of a DID can cryptographically prove that they are associated with that DID. A DID Document must include authentication key.

```c
int DIDDocumentBuilder_RemoveAuthorizationKey(DIDDocumentBuilder *builder,
        DIDURL *keyid);
```
该方法移除指定的Authorization Key。若该Key不存在或者非Authorization Key，则报错。

```c
int DIDDocumentBuilder_AddCredential(DIDDocumentBuilder *builder,
        Credential *credential);
```
该方法用于添加用户提供的Verifiable Credential。若该Credential的Id在DID Document 中已经存在，则报错。

```c
typedef struct Property {
    char *key;
    char *value;
} Property;

int DIDDocumentBuilder_AddSelfProclaimedCredential(DIDDocumentBuilder *builder,
        DIDURL *credid, const char **types, size_t typesize,
        Property *properties, int propsize, time_t expires, DIDURL *signkey, const char *storepass);
```
该方法用于直接生成并添加自声明凭证。

`types`为type数组，`typesize`为type数量；`properties`为凭证主题的数组，是凭证最主要的内容，`propsize`是凭证主题数量。

目前SDK支持五种type：SelfProclaimedCredential，EmailCredential，ProfileCredential，SocialCredential和WalletCredential，后续根据需求做适当调整。用户可以根据需要选择type，也可以通过相应方法添加自定义的type。

`expires`为有效期，不可以大于所属DID的Document有效期。

```c
int DIDDocumentBuilder_RemoveCredential(DIDDocumentBuilder *builder,
        DIDURL *credid);
```
该方法移除指定的Credential。若不存在则报错。

```c
int DIDDocumentBuilder_AddService(DIDDocumentBuilder *builder,
        DIDURL *serviceid, const char *type, const char *endpoint,
        Property *properties, int size);
```
```c
int DIDDocumentBuilder_AddServiceByString(DIDDocumentBuilder *builder,
        DIDURL *serviceid, const char *type, const char *endpoint,
        const char *properties);
```

以上两个方法都是为了添加Service，但是导入属性的方式不同。若指定id已经存在，则报错。

endpoint为Service服务点地址；properties是用户可自定义添加的内容。

```c
int DIDDocumentBuilder_RemoveService(DIDDocumentBuilder *builder,
        DIDURL *serviceid);
```
该方法移除指定的Service，若不存在则报错。

```c
int DIDDocumentBuilder_SetExpires(DIDDocumentBuilder *builder, time_t expires);
```
DID Document的有效期默认为创建之时起后推五年。如果用户也可以通过该方法自行定义有效期。该方法设置的有效期不可超过五年，否则报错。











