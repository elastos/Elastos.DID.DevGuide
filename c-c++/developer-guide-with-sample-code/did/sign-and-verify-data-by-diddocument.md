# sign and verify data by diddocument

## Sign and verify data by DID Document

DID Document可以代表DID对数据签名，因此DID Document提供方法用于签名和验签。

## Usage

```c
int DIDDocument_Sign(
        DIDDocument *document,
        DIDURL *keyid,
        const char *storepass,
        char *sig,
        int count, ...);
```

该方法使用`document`的指定`keyid`对数据签名，签名数据存放在用户给定的内存`sig`中。

`keyid`若为NULL，则默认使用Default Key；`sig`是`MAX_SIGNATURE_LEN`大小的内存；

该方法以变参方法输入需要签名数据，`count`是数据个数。

返回值为0为签名成功， -1为签名失败。

```c
int DIDDocument_SignDigest(
        DIDDocument *document,
        DIDURL *keyid,
        const char *storepass,
        char *sig,
        uint8_t *digest,
        size_t size);
```

该方法对摘要`digest`签名。`digest`为存放摘要的内存，`size`为存放摘要内存的大小。

`sig`为用户提供的存放签名数据的内存。

该方法是用于TransferTicket多签。按照Customized DID Document多签规则，用于第二个以后的Controller DID Document签名。具体示例应用可见Transfer DID。

```c
int DIDDocument_VerifyDigest(
        DIDDocument *document,
        DIDURL *keyid,
        char *sig,
        uint8_t *digest, size_t size);
```

该方法对摘要验证签名。`keyid`若为NULL，则默认使用Default Key。

```c
int DIDDocument_Verify(
        DIDDocument *document,
        DIDURL *keyid,
        char *sig,
        int count, ...);
```

该方法用于验证签名，`keyid`，`sig`和输入的数据三者其一不匹配就失败，主要防止数据被篡改。
