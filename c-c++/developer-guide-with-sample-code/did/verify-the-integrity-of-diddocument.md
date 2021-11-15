# Verify the integrity of DIDDocument

为了安全和链上文档的统一，需要检查DID Document内容本身是否有效，其是否过期，是否失效，以检查文档的可用性。

# Usage

```c
int DIDDocument_IsGenuine(DIDDocument *document);
```
DID Document提供检查文档是否完整，文档各元素是否符合要求，文档是否可验签，未被篡改的方法。比如Customized DID Dodument的signature个数是否符合多签规则。

 *      return value = -1, if error occurs;
 *      return value = 0, did isn't genuine;
 *      return value = 1, did is genuine.

```c
int DIDDocument_IsExpired(DIDDocument *document);
```
DID Document提供检查DID Document是否过期的方法。

 *      return value = -1, if error occurs;
 *      return value = 0, did isn't expired;
 *      return value = 1, did is expired.

```c
int DIDDocument_IsDeactivated(DIDDocument *document);
```
DID Document提供检查DID Document是否已经失效的方法。失效非过期，失效需要DID自己或者委托者来操作失效文档，而过期是根据有效期判断是否超出有效期范围，可再重置恢复。

```c
int DIDDocument_IsValid(DIDDocument *document);
```
DID Document提供全面检查DID Document是否有效的方法，其包含了以上三条检查。

 *      return value = -1, if error occurs;
 *      return value = 0, diddocument isn't valid;
 *      return value = 1, diddocument is valid;
