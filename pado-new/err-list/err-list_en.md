## Error Codes

We have defined some error codes in the SDK. When an error occurs during the data verification process, you can refer to the following list for troubleshooting.

### 1. General errors

| Error Code | Situation |
| :-: | :-: |
| 00001 | The MPC-TLS algorithm has not been initialized. Please restart the process. |
| 00002 | The process did not respond within 5 minutes. |
| 00003 | A data verification process is currently being generated. Please try again later. |
| 00004 | The user closes or cancels the attestation process. |
| 00005 | Wrong parameters! |
| 00006 | No extension version 0.3.15 or above was detected as installed. |
| 00007 | Insufficient wallet balance. |
| 00008 | Failed to submit the proof on-chain. Or other errors in the Wallet operations. |
| 00009 | Your dApp is not registered. Please contact the Primus team. |
| 99999 | Undefined error. Contact the Primus team for further support |


### 2. Data source related errors


| Error Code | Situation |
| :-: | :-: |
| 00102 | Attestation requirements not met. Insufficient assets balance in Binance Spot Account.。 |
| 00104 | Attestation requirements not met. |

### 3. MPC-TLS related errors


| Error Code | Situation |
| :-: | :-: |
| 10001 | The internet condition is not stable enough to complete the data verification flow. Please try again later. |
| 10002 | The attestation process has been interrupted due to some unknown network error. Please try again later. |
| 10003 | Can't connect attestation server due to unstable internet condition. Please try again later. |
| 10004 | Can't connect data source server due to unstable internet condition. Please try again later. |
| 20005 | Can't complete the attestation due to some workflow error. Please try again later. |
| 30001 ～ 30004 | Can't complete the attestation flow due to response error. Please try again later. |
| 50007 | Can't complete the attestation due to algorithm execution issues. |
| 50008 | Can't complete the attestation due to abnormal execution results. |
| 50009 | The algorithm service did not respond within 5 minutes. |
| 50010 | Can't complete the attestation due to some compatibility issues. |
| 50011 | Can't complete the attestation due to algorithm version issues. |


For any other error codes not mentioned here, please contact our [community](https://discord.gg/AYGSqCkZTz) for further support.


