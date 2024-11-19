# Frontend Example: Integrating MPC-TLS SDK in a Application

This guide will walk you through the fundamental steps to integrate Primus's proof verification system into your application.

## 1. Prerequisites
Before you begin, ensure you have:

- Installed the SDK (see [Installation Guide](../install/install_en.md))

## 2. InitAttestation
To integrate MPC-TLS SDK requires initialization

```
import MPCTLSJSSDK from "@padolabs/mpctls-js-sdk";

const sdkInstance = new MPCTLSJSSDK();

try {
  const initAttestaionResult = await sdkInstance.initAttestation("yourdAppSymbol");
  console.log(initAttestaionResult); //Output: 0.3.15
} catch (e) {
  alert(`Initialize failed,code: ${e.code} ,message: ${e.message}`);
}
```

## 3. Start MPC-TLS process
Note: You can call startAttestation only after the initAttestation method is called.

Before starting the data verification process, a few parameters should be configured and transmitted to the MPC-TLS SDK. This configuration is required regardless of how you set up your users' operation steps in you dApp.
  
The parameters should be configured in the following order:

- 1.chainID (must)
- 2.walletAddress (must)
- 3.attestationTypeID (must)
- 4.attestationParameters (must, according to different attestationTypeID)

Parameters detail you can see: [Parameters details](./params_en.md)

```
try {
  const startAttestaionResult = await sdkInstance.startAttestation({
    chainID: 56,
    walletAddress: '0x',
    attestationTypeID: '9',
    attestationParameters: ['100'],
  });
  
  console.log(startAttestaionResult); // Output:
  // {
  //   eip712MessageRawDataWithSignature:
  //     {
  //       types: {
  //         Attest: [
  //           {
  //             name: "schema",
  //             type: "bytes32",
  //           },
  //           {
  //             name: "recipient",
  //             type: "address",
  //           },
  //           {
  //             name: "expirationTime",
  //             type: "uint64",
  //           },
  //           {
  //             name: "revocable",
  //             type: "bool",
  //           },
  //           {
  //             name: "refUID",
  //             type: "bytes32",
  //           },
  //           {
  //             name: "data",
  //             type: "bytes",
  //           },
  //           {
  //             name: "deadline",
  //             type: "uint64",
  //           },
  //         ],
  //       },
  //       primaryType: "Attest",
  //       message: {
  //         schema: "0x",
  //         recipient: "0x",
  //         expirationTime: 0,
  //         revocable: true,
  //         data: "0x",
  //         refUID:
  //           "0x0000000000000000000000000000000000000000000000000000000000000000",
  //         deadline: 0,
  //       },
  //       domain: {
  //         name: "xxx",
  //         version: "xxx",
  //         chainId: "xxx",
  //         verifyingContract: "0x",
  //         salt: null,
  //       },
  //       uid: null,
  //       signature: {
  //         v: 28,
  //         r: "0x",
  //         s: "0x",
  //       },
  //     }
  // };
  console.log("Attest successfully!");
} catch (e) {
  alert(`Attest failed,code: ${e.code} ,message: ${e.message}`);
}
```

## 4. Verify attestation result

After receiving the verified result, you need to verify whether the result is trustworthy.

- **Parameters**

    - **startAttestationReturnParams:StartAttestationReturnParams** An object containing the properties of `eip712MessageRawDataWithSignature`, which is the return value of the `startAttestation` method.
- **Return:boolean** Whether the signature is successfully verified.

- **Example**

```
const verifyAttestationResult = sdkInstance.verifyAttestation(
  startAttestaionResult
);
console.log(verifyAttestation); 
// Output: true
```

## 5. Submit the verified data result (proof) to the blockchain

You can only submit the proof to the chainID associated with the configuration in **Step 3.Start MPC-TLS process**.

- Parameters

    - **startAttestationReturnParams:StartAttestationReturnParams** An object containing the properties of `eip712MessageRawDataWithSignature`, which is the return value of the `startAttestation` method.
- **wallet:any** The wallet object
- **Return:string** Transaction details URL

- **Example**

```
try {
  const sendToChainResult = sdkInstance.sendToChain(
    startAttestaionResult,
    window.ethereum
  );
  console.log(sendToChainResult); 
  // Output: https://bascan.io/attestation/0x
  console.log("SendToChain successfully!");
} catch (e) {
  alert(`SendToChain failed,code: ${e.code} ,message: ${e.message}`);
}
```

## 6.Check the submitted proof on-chain

After completing the submission, you can find the on-chain details using the corresponding blockchain links below.

### 6.1 Mainnet

- Linea Mainnet：[https://lineascan.build/](https://lineascan.build/)
- BNB Chain：[https://bascan.io/](https://bascan.io/)
- opBNB：[https://scan.sign.global/](https://scan.sign.global/)
- Arbitrum：[https://arbitrum.easscan.org/](https://arbitrum.easscan.org/)
- Scroll Mainnet：[https://scrollscan.com/](https://scrollscan.com/)

### 6.2 Testnet

- Sepolia: [https://sepolia.easscan.org/](https://sepolia.easscan.org/)
- BNBTestnet:[ https://testnet.bascan.io/]( https://testnet.bascan.io/)
- opBNBTestnet: [https://testnet-scan.sign.global/](https://testnet-scan.sign.global/)
- Scroll Sepolia: [https://sepolia.scrollscan.com/](https://sepolia.scrollscan.com/)



## 7. Examples of the MPC-TLS SDK operations
Here's a simple example demonstrating how to perform basic operations with the MPC-TLS SDK.

```
import MPCTLSJSSDK from "@padolabs/mpctls-js-sdk";

const sdkInstance = new MPCTLSJSSDK();

try {
  const initAttestaionResult = await sdkInstance.initAttestation(
    "yourdAppSymbol"
  ); // Initialize the SDK
  console.log(initAttestaionResult); 
  //Output: 0.3.15
  console.log(sdkInstance.supportedChainList); // View supported chains
  console.log(sdkInstance.supportedAttestationTypeList); // View supported attestation types

  // Generate attestation process
  const startAttestaionResult = await sdkInstance.startAttestation({
    chainID: 56, // Select from the supported chain list
    walletAddress: "0x", // User's wallet address
    attestationTypeID: "9", // Select from the supported attestation types
    attestationParameters: ["100"], // Input the corresponding fields （if needed) according to the selected attestationTypeID
  });

  // Verify attestation result
  const verifyAttestationResult = await sdkInstance.verifyAttestati(startAttestaionResult);

  // Upload Proof to Blockchain
  const sendToChainResult = await sdkInstance.sendToChain(
    startAttestaionResult,
    window.ethereum
  );

  console.log("Generated Proof:", startAttestaionResult);
  console.log("Proof on Chain:", sendToChainResult);
  console.log("Is Proof Valid:", verifyAttestationResult);
} catch (e) {
  alert(`Failed, code: ${e.code} , message: ${e.message}`);
}
```

