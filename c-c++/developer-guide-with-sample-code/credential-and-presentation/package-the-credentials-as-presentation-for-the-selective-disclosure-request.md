# Package the credentials as presentation for the selective disclosure request

可验证表达是指包含实体可验证凭证子集及副署签名(countersign)的数据集合，用于对第三方表明自身身份。

Verifiable expression refers to a data set containing a subset of verifiable credentials and countersign of an entity, which is used to show its identity to a third party.

可验证表达也可不含可验证凭证，为一个空的表达实体。

Verifiable expression can also be an empty expression entity without verifiable credential.

## Example

```c
//'doc' is holder's document
DID *holder = DIDDocument_GetSubject(doc);
DIDURL *id = DIDURL_NewFromDid(holder, "vp1");
const char *types[2] = {"Trail", "TestPresentation"};

//create Presential by four Credential objects
Presentation *vp = Presentation_Create(id, holder, types, 2, "873172f58701a9ee686f0630204fee59",
                         "https://example.com/", NULL, store, storepass, 4, cred1, cred2, cred3, cred4);
                                       
DIDURL_Destroy(id);
... ... ... ...
Presentation_Destroy(vp);
```

## Usage

```c
Presentation *Presentation_Create(
        DIDURL *id,
        DID *holder,
        const char **types, size_t size,
        const char *nonce,
        const char *realm,
        DIDURL *signkey,
        DIDStore *store,
        const char *storepass,
        int count, ...);
```

```c
Presentation *Presentation_CreateByCredentials(
        DIDURL *id,
        DID *holder,
        const char **types, size_t size,
        const char *nonce,
        const char *realm,
        Credential **creds, size_t count,
        DIDURL *signkey,
        DIDStore *store,
        const char *storepass);
```

以上两种方法都是生成Presentation，只是Credential的导入方式不同；前者是以变参形式导入，后者以数组形式导入。

Presentation can be generated through the above two methods, with different ways of Credential importing. The former is imported as a variable parameter, while the latter is imported as an array.

`id`是Presentation的id；`holder`是Presentation的持有者；`nonce`签名操作使用的随机值；`realm`表明该presentation适用的领域和地址；`signkey`是Presentation的持有者用来签名封装Presentation的Authentication Key；`count`是导入的Credential数量。

_id_ is the id of Presentation; _holder_ is the holder of Presentation; _nonce_ is a random value used by the signing; _realm_ indicates the applicable domain and address of the presentation; _signkey_ is the Authentication Key used by the holder of the Presentation to sign and encapsulate the Presentation; _count_ is the number of imported Credentials.
