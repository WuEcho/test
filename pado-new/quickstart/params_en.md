# Params Details


### 1. chainID (number)
The ID of the blockchain to which you want users to submit their proof.

```
console.log(sdkInstance.supportedChainList); 
// text: 'BlockChainName', value: chainID
// Output: [
// {text: 'Linea', value: 59144 },
// {text: 'BNB Chain', value: 56 },
// {text: 'opBNB', value: 204 },
// {text: 'Arbitrum', value: 42161 },
// {text: 'Scroll', value: 534352 },
// {text: 'Sepolia', value: 11155111 },
// {text: 'BNB Testnet', value: 97 },
// {text: 'opBNB Testnet', value: 5611 },
// {text: 'Scroll Sepolia', value: 534351 },
// ]
```
>
> NOTE
> To test your integration and business workflow, simply input the chainID parameter using one of the four test networks we support: Sepolia, BSC Testnet, opBNB Testnet, and Scroll Sepolia.
>

### 2.walletAddress (string)

The wallet address of the user. This address will be used as an index for queries on the blockchain.

### 3.attestationTypeID (string)
We have assigned different IDs to each attestation type, which can be transmitted to initialize the associated data verification process.

```
console.log(sdkInstance.supportedAttestationTypeList); 
// text:  'A function of an application.', value: 'attestationTypeID'
// Output: [
// {text: 'binance kyc status', value: '1' },
// {text: 'binance account ownership', value: '2' },
// {text: 'x account ownership', value: '3' },
// {text: 'okx kyc status', value: '4' },
// {text: 'tiktok account ownership', value: '6' },
// {text: 'binance assets balance', value: '9' },
// {text: 'binance token holding', value: '10' },
// {text: 'okx assets balance', value: '11' },
// {text: 'okx token holding', value: '12' },
// {text: 'X social connections', value: '15' },
// {text: 'binance spot 30d trade vol', value: '16' },
// {text: 'okx spot 30d trade vol', value: '17' },
// {text: 'On-chain transactions on BNB Chain since 2024 July', value: '101' },
// ]
```

### 4.attestationParameters (array)

#### 4.1 For the attestationTypeID **1, 2, 3, 4, and 6**, you need to transmit a default value [] in attestationParameters, like this:
~~~
  {
    chainID: 56,
    walletAddress: '0x',
    attestationTypeID: '1',
    attestationParameters: []
  }
~~~

#### 4.2 For attestationTypeID **9, 10, 11, 12, 15, 16, 17, and 101**, you need to transmit with different inputs.

- （1）For attestationTypeID 9 & 11
    The attestationParameters should include a USD value (numeric), with a minimum value of 0.000001 and restricted to a 6-decimal-place. If the attestationParameters is set to `['100']`, it will complete a data verification process to verify if the user's **asset balance is greater than USD 100**.

~~~
  {
    chainID: 56,
    walletAddress: '0x',
    attestationTypeID: '11',
    attestationParameters: ['100'],
  }
~~~

- （2）For attestationTypeID 10 & 12
The attestationParameters should include a token name (alphabet). If the attestationParameters is set to `['USDT']`, it will complete a data verification process to verify if the user holds **USDT equivalent to more than USD 0.1**.

~~~
  {
    chainID: 56,
    walletAddress: '0x',
    attestationTypeID: '10',
    attestationParameters: ['USDT'],
  }
~~~

- （3）For attestationTypeID 15
The attestationParameters should include a follower number (numeric), with a minimum value of 0. If the attestationParameters is set to `['10']`, it will complete a data verification process to verify if the user has **more than 10 X followers**.

```
  {
    chainID: 56,
    walletAddress: '0x',
    attestationTypeID: '15',
    attestationParameters: ['10'],
  }
```
    
- （4）For attestationTypeID 16 & 17
The attestationParameters should include a USD value (numeric), with a minimum value of 0.000001 and restricted to a 6-decimal-place. If the attestationParameters is set to ['500'], it will complete a data verification process to verify if the user's **spot 30-day trade volume is greater than USD 500**.

```
  {
    chainID: 56,
    walletAddress: '0x',
    attestationTypeID: '16',
    attestationParameters: ['500'],
  }
```
    
- （5）For attestationTypeID 101

This attestation integrates the [Brevis'](https://docs.brevis.network/) SDK to enable on-chain transaction proof, allowing verification of whether a user has conducted on-chain transactions on the BNB Chain since July 2024.

The attestationParameters should include a signature from the user’s wallet address, which needs to be attested to confirm ownership of the address, along with a timestamp indicating the time of the signature. **The input should follow this order: first, ‘user signature’; second, ‘timestamp’**.


```
  {
    chainID: 56,
    walletAddress: '0x',
    attestationTypeID: '101',
    attestationParameters: ['0xxx....', '1728546495272'],
  }
```
   
    
>
> NOTE
> In order to confirm that the user truly owns the address to be attested, you must verify that the transmitted ‘user signature’ was signed from that address. The verification method is:
> 
> 
>>  import { ethers } from "ethers";
>>
>>  const connectedAddress = '0x'
>>  const timestamp = +new Date() + ""; // '1728546495272'
>>  const provider = new ethers.providers.Web3Provider(window.ethereum);
>>
>>  const typedData = {
>>    types: {
>>      EIP712Domain: [{ name: "name", type: "string" }],
>>      Request: [
>>        { name: "desc", type: "string" },
>>        { name: "address", type: "string" },
>>        { name: "timestamp", type: "string" },
>>      ],
>>    },
>>    primaryType: "Request",
>>    domain: {
>>      name: "PADO Labs",
>>    },
>>    message: {
>>     desc: "PADO Labs",
>>      address: connectedAddress,
>>      timestamp,
>>    },
>>  };
>>
>>  const userSignature = await provider.send("eth_signTypedData_v4", [
>>    connectedAddress,
>>    typedData,
>>  ]);
>>  console.log("userSignature", userSignature); // 0xxx....
> 
>  
>


