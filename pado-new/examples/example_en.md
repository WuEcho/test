## Data Verification Capabilities Example

In this initial version, the attestable data source, attestation content, and supported blockchains are pre-set. **A future developer platform will allow customization.**

### Data Verification Capabilities

#### 1.Attestable Details

#####（1）Internet Data

| Data Sources | Attestation Content | Input Value | Attestation Result |
| --- | --- | --- | --- |
| [https://www.binance.com](https://www.binance.com/) | Asset balance in the Spot account| USD value, numeric, minimum value of 0.000001, restricted to a 6-decimal-place number | if the "attestation content" is greater than "input value" |
|  | Token holding in the Spot account | Token name, alphabet | if the "input value" is equivalent to more than USD 0.1 |
|  | Spot 30-day trade volume | USD value, numeric, minimum value of 0.000001, restricted to a 6-decimal-place number |if the "attestation content" is greater than "input value" |
|  | 	KYC Status | N/A | if the "attestation content" passed basic KYC verification |
|  | Account ownership| N/A | if the "attestation content" owns the associated account |
| [https://www.okx.com](https://www.okx.com/) |Total asset balance | USD value, numeric, minimum value of 0.000001, restricted to a 6-decimal-place number | if the "attestation content" is greater than "input value" |
|  | Token holding | Token name, alphabet | if the "input value" is equivalent to more than USD 0.1 |
|  | 	Spot 30-day trade volume | USD value, numeric, minimum value of 0.000001, restricted to a 6-decimal-place number | if the "attestation content" is greater than "input value" |
|  | KYC Status | N/A | if the "attestation content" passed basic KYC verification |
| [https://www.tiktok.com](https://www.tiktok.com/) | Account ownership | N/A | if the "attestation content" owns the associated account|
| [https://www.x.com](https://www.x.com/) | Account ownership | N/A	  | if the "attestation content" owns the associated account |
|    | Social connections | Followers number, numeric, minimum value of 0 |  value of 0	if the "attestation content" is greater than "input value" |


#####（2）Web3 Data

Integrate the [Brevis'](https://docs.brevis.network/) SDK to enable on-chain transaction proof, allowing verification of whether a user has had on-chain transactions on the BNB Chain since July 2024.

#### 2.Supported Blockchains
For the attestation contract, we currently deployed EAS and Verax attestation schemas to the following blockchains:

- Linea
- BNB Chain
- opBNB
- Arbitrum
- Scroll

If you have further needs for other blockchains, please contact us through our [community](https://discord.gg/AYGSqCkZTz) for support.

