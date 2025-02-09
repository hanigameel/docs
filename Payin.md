\# Pay-in API Documentation

\## Introduction

The Pay-in process describes how a customer makes a cryptocurrency
payment at checkout. This document outlines the API endpoints and
webhook events required to integrate Pay-in functionality into your
system.

\## Pay-in Flow Steps

\### 1. Customer Checkout & Crypto Selection

\- The customer reaches the checkout page and selects the cryptocurrency
payment option. - They choose their preferred currency and blockchain
network.

\### 2. Quote Creation & Deposit Wallet Handling

\- The system calls the \*\*Create Payin Quote\*\* API to generate a
quote (if using the quote-based model). - If using the Channel Flow, the
system checks if a deposit wallet exists:  - If not, it calls the
\*\*Create Deposit Wallet\*\* API to generate a wallet address.  - If a
wallet exists, the system proceeds to display the wallet details.

\### 3. Customer Payment & Transaction Detection

\- The system displays the wallet address or QR code for the customer to
make a payment. - The customer completes the payment within the given
time frame. - Blockchain monitoring starts to detect the transaction.

\### 4. Webhook Event Handling & Transaction Verification

Multiple webhook events track the transaction\'s progress:

\- \*\*TXN\\\_CREATED\*\* -- A new transaction is detected. -
\*\*EXPIRED\*\* -- The payment time has expired. - \*\*CREATED\*\* --
The transaction is initiated. - \*\*INITIATED\*\* -- The transaction is
in process. - \*\*COMPLIANCE\\\_REVIEW\*\* -- The transaction is under
AML/compliance screening. - \*\*RFI\*\* -- Additional information is
requested. - \*\*PAID\*\* -- The payment has been confirmed. -
\*\*REJECTED/FROZEN\*\* -- The payment is rejected or frozen due to
compliance issues. - \*\*UNDERPAID/OVERPAID\*\* -- The customer sent an
incorrect amount.

\### 5. Transaction Confirmation & Compliance Checks

\- The blockchain detects the transaction, triggering AML and compliance
screening. - If the correct amount is received and passes screening, the
transaction is confirmed. - If the payment fails compliance checks,
funds may be held, rejected, or require additional information.

\### 6. Final Settlement

\- Once approved, the funds are released and made available for the
merchant.

\## Pay-in Lifecycle Webhooks

Webhooks are sent at different stages of the transaction.

\### 1. Payin Created

Webhook Event: \*\*Payins.CREATED\*\*

\`\`\`json { \"event\": { \"orgId\": 10, \"entity\": \"GatewayPayins\",
\"numRetries\": 0, \"updatedAt\": \"2023-12-14T12:35:02.894Z\",
\"createdAt\": \"2023-12-14T12:35:02.894Z\" }, \"data\": {
\"clientIdentifier\": \"sherlockholmes\", \"address\":
\"0x51980d9a87f5de7e1DcdBe2284C39D96eC4C4361\", \"chain\": \"POLYGON\",
\"network\": \"AMOY\", \"clientOrderId\":
\"47e3bf05-1a91-4f3e-a6b4-3ac99c82eae3\", \"status\": \"CREATED\",
\"symbol\": \"USDC_USD\", \"quantity\": 1000.0, \"quoteQuantity\":
1011.0, \"fee\": 0.01, \"vat\": 0.0005, \"expiryTime\": 1736849148122 }
} \`\`\`

\### 2. Transaction Recorded on Blockchain

Webhook Event: \*\*Payins.TXN\\\_CREATED\*\*

\### 3. Transaction Initiated

Webhook Event: \*\*Payins.INITIATED\*\*

\### 4. Transaction Confirmed

Webhook Event: \*\*Payins.PAID\*\*

\### 5. Compliance Review Required

Webhook Event: \*\*Payins.COMPLIANCE\\\_REVIEW\*\*

\### 6. Additional Information Required

Webhook Event: \*\*Payins.RFI\*\*

\### 7. Transaction Rejected

Webhook Event: \*\*Payins.REJECTED\*\*

\### 8. Funds Frozen

Webhook Event: \*\*Payins.FROZEN\*\*

\## Webhook Summary

\| Status \| Description \| \|
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- \|
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--
\| \| \*\*CREATED\*\* \| Payin created. \| \| \*\*TXN\\\_CREATED\*\* \|
Transaction recorded on the blockchain. \| \| \*\*INITIATED\*\* \|
Transaction initiated, AML check started. \| \|
\*\*COMPLIANCE\\\_REVIEW\*\* \| Manual compliance review required. \| \|
\*\*RFI\*\* \| Additional information requested. \| \| \*\*REJECTED\*\*
\| Transaction rejected, funds to be returned. \| \| \*\*FROZEN\*\* \|
Funds frozen due to compliance issues. \| \| \*\*PAID\*\* \| Transaction
successfully completed. \|

\## API Reference

\### Create a Payin Quote (Optional)

\*\*Endpoint:\*\*

\`\`\` POST
https://staging.api.fuze.finance/api/v1/payment/gateway/payin/create
\`\`\`

\*\*Request Parameters:\*\*

\| Parameter \| Description \| \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- \|
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- \| \|
clientIdentifier \| Counterparty identifier. \| \| symbol \| Currency
for payment. \| \| quantity \| Payment amount. \| \| chain \| Blockchain
used. \| \| clientOrderId \| (Optional) Idempotency key. \|

\*\*Sample Request:\*\*

\`\`\`json { \"clientIdentifier\": \"barbara_allen_2\", \"symbol\":
\"USDC_USD\", \"chain\": \"POLYGON\", \"quantity\": 1000,
\"clientOrderId\": \"5468bbb7-5e5f-425c-a6eb-b89e19a0298a\" } \`\`\`

\*\*Sample Response:\*\*

\`\`\`json { \"code\": 200, \"data\": { \"id\": 107,
\"clientIdentifier\": \"barbara_allen_2\", \"clientOrderId\":
\"5468bbb7-5e5f-425c-a6eb-b89e19a0298a\", \"symbol\": \"USDC_USD\",
\"quantity\": 1000, \"address\": \"0x8f8e8b3b8b1f3f1f3f1f3f1f3f1f3f1f\",
\"chain\": \"POLYGON\", \"network\": \"AMOY\", \"expiryTime\":
\"2023-06-09T07:53:12.658Z\", \"status\": \"OPEN\" }, \"error\": null }
\`\`\`

\### List Payins

\*\*Endpoint:\*\*

\`\`\` GET
https://staging.api.fuze.finance/api/v1/payment/gateway/payin/list/
\`\`\`

\### Fetch a Payin

\*\*Endpoint:\*\*

\`\`\` GET
https://staging.api.fuze.finance/api/v1/payment/gateway/payin/status/{clientOrderId}
\`\`\`
