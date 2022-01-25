# Simulated ID chain

Because of the need for development, Java SDK has a built-in simulated ID sidechain. The back-end logic of the simulated ID sidechain is completely consistent with the implementation of Elastos ID sidechain, and provides a set of REST API based on HTTP, which is especially convenient for testing and verification during development.

In addition, there are important differences between Simulated ID chain and Elastos ID sidechain:

* The Simulated ID chain is implemented based on local memory structure - there's no need for the confirmation time of the ID sidechain, so the ID transaction will take effect immediately.
* The Simulated ID chain uses HTTP API to provide external services without a wallet, gas fees, etc.
* The Simulated ID chain has no data persistence and is only meant for development.

## Start and Use the Simulated ID Chain in the Program

```java
SimulatedIDChain simChain = new SimulatedIDChain();
simChain.start();
DIDAdapter adapter = simChain.getAdapter();

DIDBackend.initialize(adapter);
```

According to the above example, if the Simulated ID chain is started and the DID Backend is initialized, the former will be used to parse and publish DID by the DID SDK. By default, the Simulated ID chain works at port 9123 of localhost. Applications can construct objects through the Simulated ID chain (String host, int port) and specify the required listening interface and port.

## Simulated ID Chain API

### Create ID transaction

* Endpoint: /idtx
* Method: POST
* Content-Type: application/json
* Request Body: Any ID Chain request payload, include DID and VerifiableCredential operations.
* Response Body: None
* Success Status: 202

### Resolve

* Endpoint: /resolve
* Method: POST
* Content-Type: application/json
* Request Body: Any DID or VC resolve/list request.
* Response Body: Resolved result.
* Success Status: 200

### Reset Data

* Endpoint: /reset
* Parameters(optional): idtxsonly, vctxsonly
* Method: POST
* Request Body: None
* Response Body: None
* Success Status: 200

### Shutdown

* Endpoint: /shutdown
* Method: POST
* Request Body: None
* Response Body: None
* Success Status: 202
