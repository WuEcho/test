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

### 3.attestationTypeID（字符串
我们为每种证明类型分配了不同的 ID，这些 ID 可以传输以初始化相关的数据验证过程。

```
console.log(sdkInstance.supportedAttestationTypeList); 
// text:  '某个应用的某个功能', value: '类型id'
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

### 4.证明参数（数组

#### 4.1 对于 `attestationTypeID` **1、2、3、4 和 6**，您需要`[]`在 attestationParameters 中传输一个默认值，如下所示：

~~~
  {
    chainID: 56,
    walletAddress: '0x',
    attestationTypeID: '1',
    attestationParameters: []
  }
~~~

#### 4.2 对于 `attestationTypeID` **9、10、11、12、15、16、17 和 101**，您需要使用不同的输入进行传输。

- （1）对于 attestationTypeID 9
    `attestationParameters` 应包含一个 `USD` 值（数字），最小值为 `0.000001`，精确到小数点后`6`位。如果`attestationParameters`设置为`['100']`，则会完成数据验证流程，验证用户的**资产余额是否大于`100`美元**。

~~~
  {
    chainID: 56,
    walletAddress: '0x',
    attestationTypeID: '11',
    attestationParameters: ['100'],
  }
~~~

- （2）对于 attestationTypeID 10
attestationParameters 中应该包含 token 名称（字母），若 attestationParameters 设置为['USDT']，则会完成一个数据验证过程，验证用户是否持有**等值于 0.1 美元以上的 USDT**。

~~~
  {
    chainID: 56,
    walletAddress: '0x',
    attestationTypeID: '10',
    attestationParameters: ['USDT'],
  }
~~~

- （3）对于 attestationTypeID 15
attestationParameters 应包含关注者数量（数字），最小值为 0。如果 attestationParameters 设置为['10']，则会完成数据验证过程，以验证用户是否拥有**超过 10 个关注者**。

```
  {
    chainID: 56,
    walletAddress: '0x',
    attestationTypeID: '15',
    attestationParameters: ['10'],
  }
```
    
- （4）对于 attestationTypeID 16
attestationParameters 应包含一个 USD 值（数字），最小值为 0.000001，精确到小数点后 6 位。如果 attestationParameters 设置为['500']，则会完成数据验证流程，验证用户的**现货 30 天交易量是否大于 500 美元**。

```
  {
    chainID: 56,
    walletAddress: '0x',
    attestationTypeID: '16',
    attestationParameters: ['500'],
  }
```
    
- （5）对于 attestationTypeID 101

该证明集成了[Brevis](https://docs.brevis.network/)的SDK 以实现链上交易证明，可以验证用户自 2024 年 7 月以来是否在 BNB 链上进行过链上交易。

attestationParameters 应包含用户钱包地址的签名（需要进行验证以确认该地址的所有权）以及表示签名时间的时间戳。**输入应遵循以下顺序：首先是“用户签名”；其次是“时间戳”**。


```
  {
    chainID: 56,
    walletAddress: '0x',
    attestationTypeID: '101',
    attestationParameters: ['0xxx....', '1728546495272'],
  }
```
   
    
>
> 笔记
> 为了确认用户确实拥有要证明的地址，您必须验证传输的“用户签名”是否来自该地址。验证方法是：
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


