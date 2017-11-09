# FirstAPI Pay With Google

The document describes how an Android application can use FirstAPI to process a Pay with Google  transaction.
The integration process first step consists of boarding the developer and the merchant into the FirstAPI platform, getting the required credentials.
Once boarded, the application can be implemented and tested by sending requests to the FirstAPI platforms, using the credentials received in the first step.
# Requirements
- Android version 4.4 (KitKat) or higher- The Pay with Google application installed on the test device- The latest version of Google Play Services (8.4.xx)
# Obtaining Credentials
The following information is provided when you register on the FirstAPI Developer Portal:
- API Key – identifies the Developer- Token – identifies the Merchant- API Secret – used to compute the HMAC signature
Refer to the [Getting Started Guide](https://github.com/payeezy/get_started_with_payeezy/blob/master/get_started_with_payeezy042015.pdf) for instructions how to register with FirstAPI and get the information to get you going.
In addition, the Developer should create an encryption key to be used when creating the Pay with Google payload. The encryption key created should be specific for Pay with Google.
When creating an encryption key, FirstAPI provides the following:
- The Public key- The Public key hash
The public key and the public key hash are provided in the Certs page of the FirstAPI Developer Portal.
Of the five items above, only the public key is required in order to communicate with Pay with Google. The other four are used in communicating with Payeezy.
# Application Flow
The typical flow of an application using Pay with Google will be as following:
 1. Interacting with the user to get the service or merchandise that the user wishes to purchase. 2. Create a Masked Wallet request 3. Displaying the "Pay with Android" button with reference to the Masked Wallet request 4. Obtaining the Masked Wallet object from Android Pay 5. Obtaining the Full Wallet object from Android Pay 6. Using the token retrieved from the Full Wallet object to send the request to the Payeezy server 7. Process the request results
The following sections describe in more detail how to use the Payeezy credentials to interact with Android Pay and with Payeezy.
Refer to the [Android Pay Developer Site](https://developers.google.com/android-pay/?hl=en) for full documentation and guides for writing an Android Pay application.
## Issuing a Masked Wallet Request
When issuing a Masked Wallet request to Android Pay, the Public Key issued by Payeezy should be provided to Android Pay which uses it to encrypt the transaction details.
To create the MaskedWalletRequest we set the following two parameters:
1. PaymentMethodTokenizationType is set to NETWORK\_TOKEN2. PaymentMethodTokenizationParameters is set with the Payeezy provided PublicKey
Sample MaskedWalletRequest Code:
```java      PaymentMethodTokenizationParameters parameters =           PaymentMethodTokenizationParameters._newBuilder_()             .setPaymentMethodTokenizationType(                     PaymentMethodTokenizationType.NETWORK_TOKEN)             .addParameter( **"** publicKey **"** , Constants.FIRST_DATA_PUBLIC_KEY)             .build();
      MaskedWalletRequest.Builder builder = MaskedWalletRequest.newBuilder()            // Other parameters           .setPaymentMethodTokenizationParameters(parameters)           .build();```## Issuing Full Wallet Request
We get the encrypted payload returned by Android Pay in the Full Wallet response, as a token. The data included in the token should be sent to the Payeezy server in order to process the transaction.
The Full Wallet request uses the Masked Wallet retrieved in the previous step.
```java      FullWalletRequest fullWalletRequest = FullWalletRequest.newBuilder()                  .setGoogleTransactionId(googleTransactionId)                  .setCart(Cart._newBuilder_()                      .setCurrencyCode(Constants.CURRENCY_CODE_USD)                      .setTotalPrice(mAmount)                      .setLineItems(lineItems)                      .build())                  .build();
      Wallet.Payments.loadFullWallet(mGoogleApiClient,                   fullWalletRequest,                   REQUEST_CODE_RESOLVE_LOAD_FULL_WALLET);```Once the Full Wallet is received, the Payment Method Token can be extracted and parsed:
```java      PaymentMethodToken token = fullWallet.getPaymentMethodToken();      String tokenJSON = token.getToken();         // Get encrypted token```The returned token will be a UTF8 encoded serialized JSON dictionary with the following keys:
| Name | Type | Description || --- | --- | --- || encryptedMessage | String(Base64) | The encrypted message. || ephemeralPublicKey | String (base64) | The ephemeral public key associated with thePrivate key used to encrypt the message. || tag | String (base64) | MAC of encryptedMessage |
For example:
```java{  "encryptedMessage": "ZW5jcnlwdGVkTWVzc2FnZQ==",  "ephemeralPublicKey": "ZXBoZW1lcmFsUHVibGljS2V5",  "tag":"c2lnbmF0dXJl"}```## Issuing the FirstAPI Request
Once the Full Wallet is received and the token extracted and parsed, the FirstAPI request can be created. Note that while the previous steps had to be completed on the mobile device, issuing the Payeezy request can be done from the mobile device or from a server that receives the Full Wallet token.
The request uses a REST POST message with a JSON payload. As mentioned before, the four remaining items issued by Payeezy will be used when issuing the request. The following table describes where the items are used:
| Name | Used in || --- | --- || API Key | The apikey HTTP header || Token | The token HTTP header || Public Key Hash | Used in the request payload || API Secret | Used to compute the HMAC. The HMAC is added to the request through three HTTP headers. |
## The FirstAPI Request
For a full explanation of the Payeezy API please refer to the FirstAPI Developer Portal at [https://developer.payeezy.com/payeezy-api/apis/post/transactions](https://developer.payeezy.com/payeezy-api/apis/post/transactions). Following is an example of a Payeezy request payload:
```{   "currency_code":"USD",   "amount":"10000",   "merchant_ref":"orderid",   "method":"3DS",   "transaction_type":"purchase",   "3DS":{      "header":{        "publicKeyHash":"QAjHE+t/4YUVEUpoyZkkuNHJ8eap4= **",**        "ephemeralPublicKey":"ZXBoZW1lcmFsUHVibGljS2V5",      },      "signature":"c2lnbmF0dXJl",      "type":"G",      "data":"ZW5jcnlwdGVkTWVzc2FnZQ=="   }}```The following table describes the contents of the request fields:
| Field | Description || --- | --- || currency\_code | Code of the currency used in the transaction. For example, USD denotes US Dollars. || amount | The transaction amount in cents. For example, 10000 is $100.00. || merchant\_ref | The merchant's order number. || method | Must be "3DS" || transaction\_type | Describes the type of transaction, for example "purchase" || publicKeyHash | The public key hash string that was provided by Payeezy. || ephemeralPublicKey | The "ephemeralPublicKey" field extracted from the token returned by Android Pay. || signature | The "tag" field extracted from the token returned by Android Pay || type | Must be "G" || data | The "encryptedMessage" field extracted from the token returned by Android Pay |
The request should include the following HTTP headers:
| Header | Description || --- | --- || apikey | The API Key provided by Payeezy || token | The token provided by Payeezy || content-type | "Application/json" || Authorization | Value computed from the Secret and the payload (see the code to compute the HMAC in the Android Pay sample application) || nonce | see the code to compute the HMAC in the Android Pay sample application || timestamp | see the code to compute the HMAC in the Android Pay sample application |
The response from the Payeezy servers describes the results of the transaction. A sample response:
```{      "correlation_id":"227.1448293118225",      "transaction_status":"approved",      "validation_status":"success",      "transaction_type":"purchase",      "transaction_id":"222078",      "transaction_tag":"2283767",      "method":"3ds",      "amount":"1222",      "currency":"USD",      "card":{         "type":"VISA",         "cardholder_name":"Not Provided",         "card_number":"XXXXXXXX",         "exp_date":"0116"},      "bank_resp_code":"100",      "bank_message":"Approved",      "gateway_resp_code":"00",      "gateway_message":"Transaction Normal"}```For an explanation of the response fields please refer to the FirstAPI Developer Portal at [https://developer.payeezy.com/payeezy-api/apis/post/transactions](https://developer.payeezy.com/payeezy-api/apis/post/transactions).
# Sample Application The sample application provided on GitHub demonstrates how FirstAPI can be used to process Pay with Google requests.  
