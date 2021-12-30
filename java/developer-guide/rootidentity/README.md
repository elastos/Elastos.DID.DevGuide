# Root Identity

Root Identity is the root identity of a user, which is only safely held by the user. Through the Root Identity, many different DIDs can be created, and the private key of these derived DIDs can also be deduced and calculated from the private key of the root identity. Thus, holding Root Identity means holding all DIDs derived from that root identity.

One Root Identity can create several DIDs, where each DID is like the mask of a user, and different masks can be used according to needs in different application scenarios. For the convenience of application processing, Root Identity can set and store its corresponding default DID through metadata, and applications can use this attribute to set and automatically select the DID used by users.
