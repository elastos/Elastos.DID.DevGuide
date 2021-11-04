---
description: 使用ElastosDIDSDK之前需要先初始化DIDBackend，DIDBackend是一个单例，使用过程只需要配置一次。
---

# initialize the DIDBackend

初始化DIDBackend需要配置YOUR-Adapter类,在YOUR-Adapter类中配置了DID Wallet的信息，方便用户发布DIDDocument到链上

创建DefaultDIDAdapter的子类：DIDExampleAdapter，这里展示了使用第三方库web3swift。

注：具体使用哪个第三个库作为构建自定义Adapter的方法，请根据自己项目的需求选择，示例中使用的web3swift库仅作为展示：

请替换示例中YOUR-CONTRACT-METHOD、YOUR-DID-Wallet-SERVER-CODE 、YOUR CONTRACT-ABI-PARAM 3个值。

```
class DIDExampleAdapter: DefaultDIDAdapter {
    var web33: web3
    var walletAddress: EthereumAddress
    let password: String
    let contractAddress: String
    let contractMethod = "YOUR-CONTRACT-METHOD"
     public init(_ rpcEndpoint: String, _ contractAddress: String, _ walletFile: String, _ walletPassword: String) {
        let walletData = FileManager.default.contents(atPath: walletFile)
        self.contractAddress = contractAddress
        self.password = walletPassword
        self.endpoint = rpcEndpoint
        let keystore = EthereumKeystoreV3(String(data: walletData!, encoding: .utf8)!)
        let keyData = try! JSONEncoder().encode(keystore!.keystoreParams)
        let address = keystore?.addresses!.first!.address
        let wallet = Wallet(address: address!, data: keyData, name: "rpcTestName", isHD: false)

        let data = wallet.data
        let keystoreManager: KeystoreManager
        if wallet.isHD {
            let keystore = BIP32Keystore(data)!
            keystoreManager = KeystoreManager([keystore])
        } else {
            let keystore = EthereumKeystoreV3(data)!
            keystoreManager = KeystoreManager([keystore])
        }
        let ethereumAddress = EthereumAddress(wallet.address)!
        _ = try! keystoreManager.UNSAFE_getPrivateKeyData(password: password, account: ethereumAddress).toHexString()
        self.web33 = web3(provider: Web3HttpProvider(URL(string: endpoint)!, network: Networks.Custom(networkID: YOUR-DID-Wallet-SERVER-CODE))!)
        super.init(rpcEndpoint)
        
        web33.addKeystoreManager(keystoreManager)
        self.walletAddress = EthereumAddress(wallet.address)! // Address which balance we want to know
        let balanceResult = try! web33.eth.getBalance(address: walletAddress)
        let balanceString = Web3Utils.formatToEthereumUnits(balanceResult, toUnits: .eth, decimals: 3)
    }
    
    override func createIdTransaction(_ payload: String, _ memo: String?) throws {
        let value: String = "1.0" // Any amount of Ether you need to send
        let contractABIParam = "YOUR CONTRACT-ABI-PARAM"
        let contractABI = contractABIParam.toJsonString()
        let contractAddress = EthereumAddress(self.contractAddress)!
        let abiVersion = 2 // Contract ABI version
        let extraData: Data = Data() // Extra data for contract method
        _ = Web3Utils.parseToBigUInt(value, units: .eth)
        let parameters: [AnyObject] = [payload] as [AnyObject]// Parameters for contract method
        var options = TransactionOptions.defaultOptions
        options.value = 0
        options.from = walletAddress
        options.gasPrice = .manual(1000000000000) // 12
        options.gasLimit = .limited(8000000) // 6
        web33.transactionOptions = options
        let contract = web33.contract(contractABI!, at: contractAddress, abiVersion: abiVersion)!
        let tx = contract.write(
            contractMethod,
            parameters: parameters,
            extraData: extraData,
            transactionOptions: options)!
        tx.transactionOptions.from = walletAddress
        tx.transactionOptions.value = 0
        do {
            let transactionSendingResult = try tx.send(password: password, transactionOptions: options)
            self.lastTxHash = transactionSendingResult.hash
            let detial = try web33.eth.getTransactionDetails(self.lastTxHash)
        } catch {
        }
    }
}


```

初始化DIDBackend

```
// 请替换rpcEndpoint、contractAddress、walletPath、walletPassword的值
let rpcEndpoint = "YOUR-RPC-ENDPOINT"
let contractAddress = "YOUR-contract-address"
let walletPath = "YOUR-WALLET-PATH"
let walletPassword = "YOUR WALLET-PASSWORD"

let adapter = DIDExampleAdapter(rpcEndpoint, contractAddress, walletPath, walletPassword) 
try DIDBackend.initialize(adapter!)
```

调用

```
//DIDBackend初始化之后，可以使用以下代码调用DIDBackend：
DIDBackend.sharedInstance()
```
