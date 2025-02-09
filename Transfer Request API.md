\# Transfer Request API

\## Overview The Transfer Request API enables users to create transfer
requests for deposit or withdrawal operations. To access this API, users
must authenticate using a Bearer Token obtained from the Login API.

\## Authorization This API requires a Bearer Token for authentication.
The token can be retrieved by making a request to the Login API.

\## Login API

\### Endpoint \*\*URL:\*\*
\`https://visionexpresstest.odoo.com/api/login/user\`

\*\*Method:\*\* \`POST\`

\### Request Payload \`\`\`json { \"params\": { \"login\":
\"alkhayyat6test+2902@gmail.com\", // Required \"password\":
\"\^\*GMSm&Y(G4v\" // Required } } \`\`\`

\### Success Response \`\`\`json { \"jsonrpc\": \"2.0\", \"id\": null,
\"result\": { \"status\": 200, \"succeed\": true, \"access_token\":
\"access_token_008630cbd7f945345d08b232f1e406197cc0ea67\" } } \`\`\`

\## Usage The \`access_token\` from the response must be included in the
Authorization header of the Transfer Request API as a Bearer Token.

\-\--

\# Payment Notes Retrieval API

\## Overview The Payment Notes Retrieval API allows authorized users to
fetch payment notes based on a specified country code and payment method
code. It ensures secure access through token-based authentication.

\## Authentication This API requires authentication via a Bearer Token
in the Authorization header. If the token is missing, invalid, or
expired, an authentication error will be returned.

\## Endpoint \*\*URL:\*\*
\`https://visionexpresstest.odoo.com/get/payment/notes\`

\*\*Method:\*\* \`POST\`

\### Headers \| Key \| Type \| Required \| Description \|
\|\-\-\-\-\--\|\-\-\-\-\--\|\-\-\-\-\-\-\-\-\--\|\-\-\-\-\-\-\-\-\-\-\-\--\|
\| Authorization \| String \| ✅ Yes \| Bearer Token for authentication
(\`Bearer \<access_token\>\`) \|

\### Request Format \*\*Type:\*\* JSON

\| Parameter \| Type \| Required \| Description \|
\|\-\-\-\-\-\-\-\-\-\--\|\-\-\-\-\--\|\-\-\-\-\-\-\-\-\--\|\-\-\-\-\-\-\-\-\-\-\-\--\|
\| country_code \| String \| ✅ Yes \| The ISO Alpha-2 country code
(e.g., EG for Egypt). \| \| payment_method_code \| String \| ✅ Yes \|
The payment method identifier (e.g., Fawri). \|

\### Example Request \`\`\`json { \"params\": { \"country_code\":
\"EG\", \"payment_method_code\": \"Fawri\" } } \`\`\`

\### Success Response (HTTP 200) \`\`\`json { \"status\": 200,
\"succeed\": true, \"data\": \[ { \"country_id\": \"Egypt\",
\"payment_method_id\": \"Fawri\", \"deposit\": 48.9, \"notes\": \[
\"010327487607 100k\", \"01090421305 100k\" \] } \] } \`\`\`

\### Error Responses \#### 1️⃣ Missing Authorization Header (HTTP 401)
\`\`\`json { \"status\": 401, \"succeed\": false, \"message\": \"Missing
or invalid Authorization header\" } \`\`\` \#### 2️⃣ Invalid Token (HTTP
401) \`\`\`json { \"status\": 401, \"succeed\": false, \"message\":
\"Invalid token\" } \`\`\`

\#### 3️⃣ Expired Token (HTTP 401) \`\`\`json { \"status\": 401,
\"succeed\": false, \"message\": \"Token has expired\" } \`\`\`

\-\--

\# Transfer Request API

\## Overview The Transfer Request API enables users to create transfer
requests for deposit or withdrawal operations. To access this API, users
must authenticate using a Bearer Token obtained from the Login API.

\## Endpoint \*\*URL:\*\*
\`https://visionexpresstest.odoo.com/create/transfer_request/vcg/non_crypto\`

\*\*Method:\*\* \`POST\`

\### Request Headers \| Key \| Type \| Required \| Description \|
\|\-\-\-\-\--\|\-\-\-\-\--\|\-\-\-\-\-\-\-\-\--\|\-\-\-\-\-\-\-\-\-\-\-\--\|
\| Authorization \| String \| ✅ Yes \| Bearer Token (\`Bearer
\<token\>\`) \| \| Content-Type \| String \| ✅ Yes \|
\`application/json\` \|

\### Request Payload \`\`\`json { \"params\": { \"request_type\":
\"deposit\", // Required (deposit or withdrawal)
\"customer_first_name\": \"ayman\", \"customer_last_name\": \"vcg\",
\"wallet_number\": \"wallet1234\", \"customer_arabic_name\":
\"ayman11fhij\", \"sugar_client_id\": \"12348s56s7a112hijs\",
\"trading_number\": \"12345d6d728w91abcdefghij\", // Required \"phone\":
\"010\", \"mobile\": \"010\", \"email\": \"010\", \"order_id\":
\"a1w24s2w2r823\", // Required ID for your order \"transaction_id\":
\"c456\", // ID for your transaction \"operating_unit_code\": \"V\", //
Required if new customer \"date\": \"21/02/2023 18:54:55.099000\",
\"source_country_code\": \"EG\", \"payment_method_code\": \"ExH\", //
Required \"payment_method_note\": \"test2222222222\",
\"source_city_code\": \"ALX\", \"local_currency_code\": \"EGP\",
\"wallet_address\": \"wallet_address\", \"account_manager\":
\"account_manager\", \"amount\": 10 } } \`\`\`

\### Success Response \`\`\`json { \"jsonrpc\": \"2.0\", \"id\": null,
\"result\": { \"status\": 200, \"succeed\": true, \"transfer\": 120523,
\"transfer_name\": \"RF/66508\", \"message\": \"transfer created
successfully\" } } \`\`\`

\-\--

\# Summary ✔ Secure API requiring authentication via Bearer Token ✔
Login API provides access token for authentication ✔ Transfer Request
API facilitates deposits and withdrawals ✔ Comprehensive error handling
for smooth integration ✔ Detailed request and response structures for
easy implementation
