# Integrity and Expiration

For safety reasons, the Elastos DID Document and verifiable credential all have an effective period. The longest effective period of DID Document can be set to 5 years, or users can set it to a shorter effective period according to their own needs. A DID topic that exceeds the effective period will be recognized as invalid by the DID parser. The longest effective period of the verifiable credential is the effective period of its holder, and if it exceeds the effective period, it is also an invalid credential. The effective period can be modified to prolong the time range if needed as well.

Elastos DID is a decentralized personal identity system where it's most important function is to ensure security under the premise of decentralization.

Elastos DID Document, verifiable credential, and verifiable presentation all need to meet certain validity conditions.
