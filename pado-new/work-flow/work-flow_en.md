# Primus Workflows

![](./image/1.png)

**The [Primus Extension](https://chromewebstore.google.com/detail/pado/oeiomhmbaapihbilkfkhmlajkeegnjhe) is required to complete the MPC-TLS process on the data source page. When using the MPC-TLS SDK, prompt users in your dApp to install the latest version (above 0.3.15) of the Extension, as it is required.**

1. User Onboarding: The user onboards to your dApp, connects their Wallet, and follows your instructions to initiate the data attestation process.

2. Configure Data Verification Parameters: Before starting the attestation process, ensure that all parameters required by the SDK for this attestation workflow are properly configured.

3. Initiate Data Verification Workflow: Your dApp activates the MPC-TLS SDK, requesting the data and attestation content configured in the SDK.

4. Redirect to the Data Source Page: Your dApp redirects the user to the data source page. After the user logs in to their account on that website, the Extension pop-up window will appear in the right corner of the data source page.

5. Start Data Verification Process: The user needs to click the start button on the Extension pop-up window to generate an attestation process. If the data source page requires a login, the user must complete the login before clicking ‘start’ button.

6. Execute MPC-TLS Protocol: Once the data verification process begins, the MPC-TLS protocol runs between the data source page, the extension, and the attestor to complete a privacy-preserving attestation process.

7.Return Attestation Result: After the MPC-TLS attestation process is complete, whether successful or not, the Extension retrieves the result and sends it to the MPC-TLS SDK. For the failed tasks, we provide several [error codes](../err-list/err-list_en.md) to help you better identify and troubleshoot issues.

8. Get and Verify the Attestation Result: Your dApp will retrieve the attestation result from the SDK and verify Primus' signature to confirm whether the result is trustworthy.

9. Business Logic: Your dApp will execute business logic based on the proof that users obtain from the SDK. Your dApp will determine how to use the proof, whether to submit it on-chain, and how to use it.

