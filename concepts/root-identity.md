# Root identity

联系中心化的世界，我们每个人都拥有多个账户多个Id去处理或者管理不同事务。在去中心化的世界里，我们可以通过拥有多个DID去满足这种需求，如何让同一个人拥有并且方便得管理多个DID呢？Elastos DID SDK采用HD钱包生成多地址的方式来实现多DID的产生和管理。

Each of us has multiple accounts and IDs to handle or manage different affairs in the centered world. While in the decentralized world, we can meet this demand by owning multiple DIDs. How can the same person own and manage multiple DIDs conveniently? Elastos DID SDK uses HD wallet to generate multiple addresses to realize the generation and management of multiple DID.

HD钱包是通过助记词来作为种子或者说是可以产生多个地址的根，对应于此，Elastos DID SDK用RootIdentity根身份来对应该种子，Elastos DID SDK 通过用户提供助记词（mnemonic）和密码（passphase）或者扩展根私钥（extended key）两种方式来获取RootIdentity。

HD wallet uses mnemonic as seeds or roots that can generate multiple addresses. Correspondingly, Elastos DID SDK utilizes root identity to correspond to seeds, and Elastos DID SDK obtains root identity by providing mnemonic and pass phase or giving expanded key.

HD钱包通过种子可以衍生出多个地址，同样，根身份也可以衍生出一系列的DID（衍生的地址作为DID字串，相对应的公钥私钥用于该DID密钥体系，可用于签名验签），这一系列的DID都被根身份的持有者管理。

HD wallet can derive multiple addresses from seeds. Similarly, the root identity can also derive a series of DIDs (the derived addresses are used as DID strings, and the corresponding public and private keys are used in the DID key system, which can be used for signature verification). These series of DIDs are managed by the holder of the root identity.

一个人可以通过一个mnemonic或者extended key来产生多个DID。

One person can generate multiple DID through one mnemonic or extended key.
