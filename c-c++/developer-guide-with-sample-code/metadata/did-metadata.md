# DID Metadata

DID Metadata是后台保存了和DID相关的不能放入DID Document的其他信息，比如昵称，链上状态，最新交易记录等。

DID Metadata属性分为默认属性和用户自定义属性。

DID Metadata默认属性分为可读写和只读两种：可读写属性可以通过get和set相应属性来读写功能，主要有alias，只读属性可以通过get相应属性来实现读的功能，主要有root identity id（即该DID从哪个根身份衍生而来），index（该DID是根身份衍生的第几个DID），transaction id（本地保存的最后一次resolve得到的交易id，此id并不一定是链上最新的transaction id，这个取决于最后一次DID Resolve的时机），published time（本地最后一次上链时间，此时间也不一定是链上最后一次交易上链时间，这个取决于本地是否更新交易或者最后一次上链是否在本地完成）和deactivate 状态。因此建议本地先更新链上最新交易再获取DID Metadata属性会更准确。具体的各属性方法详见API文档。

DID Metadata自定义属性可通过getExtra_和setExtra_来实现读写功能。具体方法详见API文档。

## Example

```typescript
DIDMetadata *metadata = DIDDocument_GetMetadata(doc);
if (!metadata)
  //error operation
  
if (DIDMetadata_SetAlias(metadata, "little-fish") < 0)
  //error operation

... ... ... ...
```

各属性的其他使用方法详见API文档。

## Usage

```c
DIDMetadata *DIDDocument_GetMetadata(DIDDocument *document);
```

该方法从DID Document中获取Metadata object。

具体从Metadata object获取具体内容，详见API文档更多方法。
