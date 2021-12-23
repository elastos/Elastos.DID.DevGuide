# Verifiable presentation

可验证表达是指包含实体可验证凭证子集及副署签名(countersign)的数据集合，用于对第三方表明自身身份。

Verifiable presentation refers to a data set containing a subset of verifiable certificates and countersign of an entity, which is used to show its identity to a third party.

一般而言，可验证表达中包含的是针对一个DID身份的凭证，这些凭证可以是不同的第三方实体颁发，用来表达实体的身份信息。

Generally speaking, verifiable presentation contains vouchers for one DID identity, which can be issued by different third-party entities to express the identity information of the entity.

Verifiable presentation可含多个凭证或者无凭证，一次成型，不可修改。

The verifiable presentation can contain multiple vouchers or no vouchers, which are formed once and cannot be modified.

Verifiable presentation只能封装自己的verifiable credential，其holder对最后结果签名。因此Verifiable presentation的holder就是verifiable credential的subject，否则Verifiable presentation无效。

The verifiable presentation can only encapsulate its own verifiable credential, and its holder signs the result. Therefore, the holder of the verifiable presentation is the subject of the verifiable credential, otherwise the verifiable presentation is invalid.
