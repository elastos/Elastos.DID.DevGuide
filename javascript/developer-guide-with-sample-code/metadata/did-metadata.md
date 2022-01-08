# DID Metadata

DID Metadata是后台保存了和DID相关的不能放入DID Document的其他信息，比如昵称，链上状态，最新交易记录等。

DID metadata refers to the other DID-related information stored in the background that cannot be put into the DID document, e.g. nickname, chain status and latest transaction records.

DID Metadata属性分为默认属性和用户自定义属性。

The attributes of DID metadata are divided into default and customized attributes.

DID Metadata默认属性分为可读写和只读两种：可读写属性可以通过get和set相应属性来读写功能，主要有alias，只读属性可以通过get相应属性来实现读的功能，主要有root identity id（即该DID从哪个根身份衍生而来），index（该DID是根身份衍生的第几个DID），transaction id（本地保存的最后一次resolve得到的交易id，此id并不一定是链上最新的transaction id，这个取决于最后一次DID Resolve的时机），published time（本地最后一次上链时间，此时间也不一定是链上最后一次交易上链时间，这个取决于本地是否更新交易或者最后一次上链是否在本地完成）和deactivate 状态。因此建议本地先更新链上最新交易再获取DID Metadata属性会更准确。具体的各属性方法详见API文档。

The default attributes of DID metadata can be divided into two types, read-write and read-only. The read-write attributes can be realized by getting and setting corresponding attributes, which mainly include alias; read-only attributes can be achieved by getting corresponding attributes, mainly including root identity id (that is, which root identity this DID is derived from), index (as an DID derived from the root identity, what place does this DID rank?), transaction id (the locally saved transaction id obtained by the last resolve, this id is not necessarily the latest transaction id on the chain, which depends on the time of the last DID resolve), published time (the time when the local data is published to the chain for the last time, and this is not necessarily the time when the last transaction is published to the chain, which depends on whether the transaction is updated locally or whether the last publishing is completed locally) and deactivated status. Hence, it is suggested that the latest transaction on the chain be updated locally before obtaining DID metadata attributes, which will be more accurate. Refer to the API document for specifics.

DID Metadata自定义属性可通过getExtra_和setExtra_来实现读写功能。具体方法详见API文档。

The customized attributes of DID metadata can be read and written by getExtra and setExtra. See the API document for details.

## Example

```typescript
let did = doc.getSubject();
let resolved = did.resolve();
let metadata = resolved.getMetadata();
metadata.setExtra("usage", "for signing in");
... ... ...
let published = metadata.getPublished();
let txId = metadata.getTransactionId();
let usage = metadata.getExtra("usage");
... ... ...
```

各属性的其他使用方法详见API文档。

The API document shows the detailed methods of using various attributes.
