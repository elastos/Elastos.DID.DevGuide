# Verify the Integrity of DIDDocument

For the sake of security and the unity of documents on the chain, it's important to check whether the DID Document content itself is valid, invalid, or expired so as to check the usability of the documents.

## Usage

```c
int DIDDocument_IsGenuine(DIDDocument *document);
```

Usage DID Document provides methods to check whether the document is complete, each element meets the requirements, can be verified, and has been tampered with. For example, whether the number of signatures of Customized DID Document meets the multi-signature rule.

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

DID Document provides a method to check whether the DID Document is valid - invalidation is not equal to expiration. It requires DID itself or the principal to operate the invalid document, while expiration is to judge whether it is beyond the effective period, which can be reset and restored.

```c
int DIDDocument_IsValid(DIDDocument *document);
```

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
