# RootIdentity

RootIdentity 是一个用户的根身份，并且仅为用户自己安全的持有。通过 RootIdentity 所代表的根身份，可以衍生创建很多不同的 DIDs，而这些衍生创建的 DIDs 的私钥也可以由该根身份的私钥推演计算出来。所以持有 RootIdentity 意味着持有所有该根身份衍生的 DIDs。

It is the root identity of a user, which is only safely held by the user. Through the Root Identity, many different DIDs can be created, and the private key of these derived DIDs can also be deduced and calculated from the private key of the root identity. Thus, holding Root Identity means holding all DIDs derived from that root identity.

因为一个 RootIdentity 可以创建很多的 DIDs，每个 DIDs 都类似于用户的面具，在不同的应用场景中，可以根据需要使用不同的面具。为了方便应用的处理，RootIdentity 可以通过 Metadata 来设定和存储其对应的默认 DID，应用可以使用这个属性来设定和自动选择用户使用的 DID。

One Root Identity can create a lot of DIDs, each DIDs is like the mask of a user, and different masks can be used according to needs in different application scenarios. For the convenience of application processing, Root Identity can set and store its corresponding default DID through Metadata, and applications can use this attribute to set and automatically select the DID used by users.
