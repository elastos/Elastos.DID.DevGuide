# Credential and Presentation

DID does not contain personal information in principle. Personal information is represented by verifiable credentials in DID system - different credentials can be created for different personal information for various application scenarios.

When presenting the credential to the verifier, the holder needs to pack it into verifiable expression, which can prevent the malicious use of the credential and ensure that even if the third party obtains it, it can’t be effectively used.

The verifiable credential can be self-declared, or one that's passed through a third-party KYC. The credential verifier can request necessary credentials as needed, including the acceptance of credential information.

The credential is a sealed object - once created, it cannot be changed. If there's a need for change, it's necessary to re-create an updated credential.
