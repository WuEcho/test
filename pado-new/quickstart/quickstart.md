# 前端示例：在应用程序中集成MPC-TLS SDK

本指南将引导您完成将 Primus 的证明验证系统集成到您应用程序的基本步骤。

## 1. 先决条件
在开始之前，请确保您已：

- 安装SDK（参见[安装指南](../install/install.md)）

## 2. 初始化
要想集成MPC-TLS SDK需要先进行初始化操作

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

## 3. 启动MPC-TLS
注意：只有调用`initAttestation`方法后才能调用`startAttestation` 。

在开始数据验证过程之前，需要配置一些参数并将其传输到 MPC-TLS SDK。无论您如何在 dApp 中设置用户的操作步骤，都需要进行此配置。
  
参数应按以下顺序配置：

- 1.chainID（必须）
- 2.钱包地址（必须）
- 3.attestationTypeID（必须）
- 4.attestationParameters（必选，根据不同的attestationTypeID）

具体参数说明可参考：[参数说明](./params.md)

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

## 4. 验证认证

当收到验证结果后，需要验证该结果是否可信。

- **参数**
    - **startAttestationReturnParams**：**StartAttestationReturnParams**一个包含 `eip712MessageRawDataWithSignature` 属性的对象，是`startAttestation`方法的返回值。 
- **返回**：**boolean**签名是否验证成功。
- **示例**

```
const verifyAttestationResult = sdkInstance.verifyAttestation(
  startAttestaionResult
);
console.log(verifyAttestation); 
// Output: true
```

## 5. 验证数据结果提交
您只能向**3.启动MPC-TLS**中配置关联的chainID提交证明。

- **参数**
    - **startAttestationReturnParams**：**StartAttestationReturnParams**一个包含 `eip712MessageRawDataWithSignature` 属性的对象，是`startAttestation`方法的返回值。
    - **wallet**:**any**钱包对象

- **返回**：**字符串**交易详情URL
- **示例**

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

## 6.链上查看提交证明

完成提交后，您可以使用下面相应的区块链链接找到链上详细信息。

### 6.1 主网

- Linea 主网：[https://lineascan.build/](https://lineascan.build/)
- BNB链：[https://bascan.io/](https://bascan.io/)
- opBNB：[https://scan.sign.global/](https://scan.sign.global/)
- Arbitrum：[https://arbitrum.easscan.org/](https://arbitrum.easscan.org/)
- Scroll 主网：[https://scrollscan.com/](https://scrollscan.com/)

### 6.2 测试网

- Sepolia: [https://sepolia.easscan.org/](https://sepolia.easscan.org/)
- BNBTestnet:[ https://testnet.bascan.io/]( https://testnet.bascan.io/)
- opBNBTestnet: [https://testnet-scan.sign.global/](https://testnet-scan.sign.global/)
- Scroll Sepolia: [https://sepolia.scrollscan.com/](https://sepolia.scrollscan.com/)



## 7. 完整示例
这是一个简单示例，演示如何使用 MPC-TLS SDK 执行基本操作。

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

