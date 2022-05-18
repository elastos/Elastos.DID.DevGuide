# Root Identity

Each of us has multiple accounts and IDs that we need to handle or manage differently - while engaging in the decentralized world, we can meet this demand through ownership of multiple DIDs.

So how can the same person simultaneously and conveniently own and manage multiple DIDs? The Elastos DID SDK uses the HD wallet to generate multiple addresses, allowing for the generation and management of multiple DIDs.

The HD wallet uses mnemonics as seeds or roots that can generate multiple addresses. Correspondingly, the Elastos DID SDK utilizes root identity to correspond to seeds - the Elastos DID SDK then obtains root identity by providing mnemonic and pass phases, or provided an expanded key.

In addition, the HD wallet can derive multiple addresses from seeds. Similarly, the root identity can also derive a series of DIDs (the derived addresses are used as DID strings, and the corresponding public and private keys are used in the DID key system, which can be used for signature verification). These series of DIDs are managed by the holder of the root identity.

One person can also generate multiple DIDs through one mnemonic or extended key.
