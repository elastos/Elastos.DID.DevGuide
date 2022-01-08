# Credential and Presentation

In principle, the DID does not include personal information which is presented by verifiable credentials in the DID system. Different credentials can be created based on different personal information to be applied to different application scenarios.

When presenting the credentials to the verifier, the credential holder needs to package them into verifiable expression, which prevents the malicious use of these credentials. It ensures that even if the third party obtains the credentials, they cannot be effectively used.

The verifiable credential can be self-proclaimed or contain the KYC specific service requirements of a third party. The verifier can request necessary credentials as needed, including the acceptance of credential information.

The credential is a sealed object which cannot be changed once it is created. If there is a need for change, an updated credential should be re-created.
