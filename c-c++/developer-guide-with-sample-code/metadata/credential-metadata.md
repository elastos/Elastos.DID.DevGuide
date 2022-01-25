# Credential Metadata

Credential Metadata is information that saved in the background and related to Credential but cannot be put into Credential in the background, such as a nickname, chain status, and latest transaction record.

Credential Metadata attributes are divided into default attribute and customized attribute.

The default attributes of Credential Metadata can be divided into read-write and read-only. The read-write attribute can be read and written by the corresponding attributes of get and set, mainly related to alias; the read-only attribute can be read by the corresponding attribute of get, mainly related to transaction ID (the locally saved transaction id obtained by the last resolve; this ID is not necessarily the latest transaction id on the chain, which depends on the time of the last Credential Resolve), published time (the last declaration time of the local, which is not necessarily the cochain time of the last transaction on the chain, which depends on whether the transaction is updated locally or whether the last cochain is completed locally), and revoked status.&#x20;

Therefore, it's suggested that it's more accurate to update the latest transaction on the chain locally and then obtain the Credential Metadata attributes. See the API documentation for the specific attributes and methods.

The customized attribute of Credential Metadata can be read and written by getExtra and setExtra. See API documentation for details.

## Example

```c
CredentialMetadata *metadata = Credential_GetMetadata(vc);
if (!metadata)
  //error operation
  
if (CredentialMetadata_SetAlias(metadata, "Email") < 0)
  //error operation

... ... ... ...
```

各属性的其他使用方法详见API文档。

See API documentation for other usages of each attribute.

## Usage

```c
CredentialMetadata *Credential_GetMetadata(Credential *credential);
```

该方法从Credential中获取Metadata object。

The method gets Metadata object from Credential.

具体从Metadata object获取具体内容，详见API文档更多方法。

Get specific content from Metadata object. See API documentation for more methods.
