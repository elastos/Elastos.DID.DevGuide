# Credential and presentation

DID 原则上不包含个人信息。个人信息在 DID 的体系中是通过可验证凭证来表现的。可以针对不同的个人信息创建不同的凭证，用于不同的应用场景。

凭证持有人在向验证方出示凭证时，需要将凭证打包为可验证表达，这样可以防止凭证的恶意使用，保证第三方即使取得了凭证，也不能有效的使用凭证。

可验证凭证可以是自声明的凭证，也可以是经过第三方 KYC 的凭证。凭证验证方可以根据需要请求必要的凭证，包括对凭证信息的采信。

凭证是 sealed 对象，一旦创建则不可更改。如果有变更的需要，那么需要重新创建一个更新的凭证。