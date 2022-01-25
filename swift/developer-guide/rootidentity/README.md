# RootIdentity

RootIdentity is the root identity of a user that's safely held only by the user themselves. Through RootIdentity, many different DIDs can be derived and created, and their respective private keys can also be deduced and calculated from the private key of the root identity. Hence, possessing RootIdentity means possessing all DIDs derived from this root identity.

A RootIdentity can create many DIDs, each of which is similar to a mask of the user who can use different masks according to the needs of different application scenarios. For the convenience of application processing, RootIdentity can set and store its corresponding default DID through metadata, and applications can use this attribute to set and automatically select the DID utilized by users.
