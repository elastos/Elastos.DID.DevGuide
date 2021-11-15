# Credential Metadata

Credential Metadata是后台保存了和Credential相关的不能放入Credential的其他信息，比如昵称，链上状态，最新交易记录等。

Credential  Metadata属性分为默认属性和用户自定义属性。

Credential  Metadata默认属性分为可读写和只读两种：可读写属性可以通过get和set相应属性来读写功能，主要有alias，只读属性可以通过get相应属性来实现读的功能，主要有transaction id（本地保存的最后一次resolve得到的交易id，此id并不一定是链上最新的transaction id，这个取决于最后一次Credential Resolve的时机），published time（本地最后一次declare时间，此时间也不一定是链上最后一次交易上链时间，这个取决于本地是否更新交易或者最后一次上链是否在本地完成）和revoked状态。因此建议本地先更新链上最新交易再获取Credential Metadata属性会更准确。具体的各属性方法详见API文档。

Credential Metadata自定义属性可通过getExtra*和setExtra*来实现读写功能。具体方法详见API文档。

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

## Usage

```c
CredentialMetadata *Credential_GetMetadata(Credential *credential);
```

该方法从Credential中获取Metadata object。

具体从Metadata object获取具体内容，详见API文档更多方法。
