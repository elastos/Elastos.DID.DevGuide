# Sign and Verify Data by DID Document

DID Document can sign data on behalf of DID, so DID Document provides methods for signing and verification.

## Usage

```c
int DIDDocument_Sign(
        DIDDocument *document,
        DIDURL *keyid,
        const char *storepass,
        char *sig,
        int count, ...);
```

This method uses the specified keyid of document to sign the data, and the signed data is saved in the memory sig given by the user.

If the keyid is NULL, it defaults to use the Default Key; _sig_ is memory of MAX \_ SIGNATURE \_ LEN size.

This method inputs the data to be signed by changing parameters, and _count_ is the number of data.

If the return value is 0, the signing is successful; if it's -1, the signing is failed.

```c
int DIDDocument_SignDigest(
        DIDDocument *document,
        DIDURL *keyid,
        const char *storepass,
        char *sig,
        uint8_t *digest,
        size_t size);
```

This method signs the digest. digest is the memory for saving abstracts, and size is the size of the memory for saving abstracts.

The memory provided by _sig_ for users to save signature data.

This method is used for TransferTicket multi-signature. According to the multi-signature rule of Customized DID Document, it is used for the second and later Controller DID Document signature. The example application can be seen in Transfer DID:

```c
int DIDDocument_VerifyDigest(
        DIDDocument *document,
        DIDURL *keyid,
        char *sig,
        uint8_t *digest, size_t size);
```

This method verifies the signature of the digest. If the keyid is NULL, the Default Key will be used by default.

```c
int DIDDocument_Verify(
        DIDDocument *document,
        DIDURL *keyid,
        char *sig,
        int count, ...);
```

This method is used to verify the signature, keyid, sig and the input data, which will fail if they do not match, and mainly prevent the data from being tampered with.
