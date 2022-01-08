# Verify the integrity of DIDDocument

## Verify the integrity of DIDDocument

为了安全和链上文档的统一，需要检查DID Document内容本身是否有效，其是否过期，是否失效，以检查文档的可用性。

For the sake of security and the unity of documents on the chain, it is necessary to check whether the DID Document content itself is valid, whether it is expired or invalid, so as to check the usability of the documents.

## Usage

```c
int DIDDocument_IsGenuine(DIDDocument *document);
```

DID Document提供检查文档是否完整，文档各元素是否符合要求，文档是否可验签，未被篡改的方法。比如Customized DID Dodument的signature个数是否符合多签规则。

Usage DID Document provides methods to check whether the document is complete, whether each element of the document meets the requirements, whether the document can be verified, and whether it has been tampered with. For example, whether the number of signatures of Customized DID Document meets the multi-signature rule.

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, did isn't genuine;
  ```
* ```
   return value = 1, did is genuine.
  ```

```c
int DIDDocument_IsExpired(DIDDocument *document);
```

DID Document提供检查DID Document是否过期的方法。

DID Document provides a method to check whether DID Document is out of date.

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, did isn't expired;
  ```
* ```
   return value = 1, did is expired.
  ```

```c
int DIDDocument_IsDeactivated(DIDDocument *document);
```

DID Document提供检查DID Document是否已经失效的方法。失效非过期，失效需要DID自己或者委托者来操作失效文档，而过期是根据有效期判断是否超出有效期范围，可再重置恢复。

DID Document provides a method to check whether the DID Document is valid. Invalidation is not equal to expiration. It requires DID itself or the principal to operate the invalid document, while expiration is to judge whether it is beyond the effective period, which can be reset and restored.

```c
int DIDDocument_IsValid(DIDDocument *document);
```

DID Document提供全面检查DID Document是否有效的方法，其包含了以上三条检查。

DID Document provides a comprehensive method to check whether DID Document is valid, which includes the above three checks.

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, diddocument isn't valid;
  ```
* ```
   return value = 1, diddocument is valid;
  ```
