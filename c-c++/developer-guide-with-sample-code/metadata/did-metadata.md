# DID Metadata

DID Metadata is the information that's saved in the background and related to DID but cannot be put into DID Document, such as a nickname, chain status, and latest transaction. DID Metadata attributes are divided into default and customized attributes.

The default attributes of DID Metadata can be divided into read-write and read-only. The read-write attribute can be read and written by corresponding attributes of get and set, mainly related to alias; the read-only attribute can be read by corresponding attributes of get, mainly related to root identity ID (that is, which root identity the DID is derived from), index (which DID the DID is derived from the root identity), transaction ID (the locally saved transaction id obtained by the last resolve - this ID isn't necessarily the latest transaction ID on the chain, which depends on the time of the last DID Resolve), published time (the last cochain time of the local, which isn't necessarily the cochain time of the last transaction on the chain, which depends on whether the transaction is updated locally or whether the last cochain is completed locally), and deactivation status.&#x20;

Therefore, it's suggested that it's more accurate to update the latest transaction on the chain locally and then obtain DID Metadata attributes. See the API documentation for the specific attributes and methods.

The customized attribute of DID Metadata can be read and written by getExtra and setExtra. See API documentation for details.

## Example

```typescript
DIDMetadata *metadata = DIDDocument_GetMetadata(doc);
if (!metadata)
  //error operation
  
if (DIDMetadata_SetAlias(metadata, "little-fish") < 0)
  //error operation

... ... ... ...
```

See API documentation for other usages of each attribute.

## Usage

```c
DIDMetadata *DIDDocument_GetMetadata(DIDDocument *document);
```

The method gets Metadata object from DID Document.

Get specific content from Metadata object. See API documentation for more methods.
