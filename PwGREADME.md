# Pay With Google
The document describes how developer can use First Data's RESTful API solution to process a Pay with Google  transaction.
The integration process first step consists of registering the developer and boarding the merchant into the API platform, and then getting the required credentials.

Refer to the [Getting Started Guide](https://github.com/payeezy/get_started_with_payeezy/blob/master/get_started_with_payeezy042015.pdf) for instructions how to register and get the information to get you going.

Once registered, the application can be implemented and tested by sending requests to the API platforms, using the credentials received in the first step.

## Requirements

- Android version 4.4 (KitKat) or higher
- The Pay with Google application installed on an Android mobile device
- The latest version of Google Play Services (11.4.x)

## Obtaining Credentials
The following information is provided when you register on the FirstAPI Developer Portal
- API Key – identifies the Developer
- Token – identifies the Merchant
- API Secret – used to compute the HMAC signature

## Application Flow
The typical flow of an application using Pay with Google will be as following:
1. On the mobile app or website, the consumer selects the service or merchandise they wish to purchase and places it into their shopping cart. 
2. The "Pay with Google" button is displayed on the checkout page of the mobile app or website to allow the consumer to select as payment method.
3. The merchant/client server issues a credential request with the Merchant ID and Processor Name as First Data to Google.
For a full explanation of the API please refer to the [Developer Portal](https://developer.payeezy.com/payeezy-api/apis/post/transactions-17).
4. Google returns response with encrypted payment credentials signed with the First Data key to the merchant server.</p>
5. The Merchant sends the encrypted payload to First Data.</p>
6. First Data decrypts and validates the payload,  and then processes the transaction and responds back to merchant with either an approval or decline response.

The following sections describe in more detail how to use the FirstAPI credentials to interact with Pay with Google and with FirstAPI.

## Requesting Credentials
To create the credential request the developer needs:

1. The First Data-issued Merchant ID
2. The Gateway Tokenization parameter, which is set to 'firstdata'

Google will return the encrypted payload to the app.

## Request Header Parameters
The API request uses the RESTful library Volley to send the request to First Data (the payment processor.) The following table describes the parameters used as the request header:

| Name | Used in |
|------|---------|
| API Key | HTTP header |
| Token | HTTP header |
| API Secret | Used to compute the HMAC. The HMAC is added to the request through the HTTP headers.|

## Sample Request Payload
The following is an example of a FirstAPI request payload:

```
{  "currency_code": "USD",  
   "amount": "001",  
   "merchant_ref": "orderid",  
   "transaction_type": "purchase",  
   "method": "3DS",  
   "3DS": {    
         "signature": "MEYCIQDfe+NNUN/FTALysbWGMAqwqQfIKZ/Pqsbjkq0thdz4zwIhAN8hybr1BPTfp0Z11UXWSXDffpM0mnbQ/MCrsQaOXgQ6",    
         "type": "G",    
         "version": "ECv1",
         "data":"{\"encryptedMessage\":\"7gUCb+nYwydKXg40H2K19/OlGIAmGUfeim5YuTOHOj8YygKpQuRbueqrtoT2V39dTBd+0eq9tqLkPit9mksGM6IwAZkbhMeuHoFFNevpRHP+9QHwYcMadsKgYv4tdHnEd3zOq8zSc63KC2FudKcHXHeiL8MwRAMSMSdOiEBJjg3ZdFS2K6HnVxuZZah1HK/w2FIIsInutS1ItPyDxm+wvmDd6ahvERsJQdUitK6S5KQ2UC4kBhdhJX6dosBybbSk89ux7hxbBYWdiCU8ARCYsFQ237YXMasajg3woWkzYxKOlqTtpm4YVoH327lwkXBgwo0CL6BTfOH3tylZLw59+XytpEEIVZdvIibpo+mm4odw/eBdFuxazlC20XaSfIOP620tyTE8lh8Qf28Aea/CNyvYXOgfDURiTEed1KlRIATKkBIwrOwsB//gmiNcuOKcEFO3jNsSlg\\u003d\\u003d\",\"ephemeralPublicKey\":\"BCzn9AukQpQXQYUax5nh4e5dl8D8az1T0XpWHd/6PssLIRq7SpWEiuO/Sr5WSPhf4SD15EtmF6zhnjD1MwciqJA\\u003d\",\"tag\":\"oL67zq3qfISY0TRp5vW7CVNPZlL3bYmV8bcIa1n6SDM\\u003d\"}"  
         }
 }
```

The following table describes the contents of the request fields:

| Field | Description |
| ----- | ----------- |
| currency_code | Code of the currency used in the transaction. For example, USD denotes US Dollars. |
| amount | The transaction amount in cents. For example, 10000 is $100.00. |
| merchant_ref | The merchant's order number. |
| transaction_type | The transaction type the merchant wants to process |
| method | Must be "3DS" |
| transaction_type | Describes the type of transaction, for example "purchase" |
| 3DS.signature | Signature for verifying that the message comes from Google |
| 3DS.type | Should be set to "G" for Pay with Google | 
| 3DS.version | Should be set to ECv1 |
| 3DS.data.encryptedMessage | | 
| 3DS.data.ephemeralPublicKey | The "ephemeralPublicKey" field extracted from the token returned by Pay with Google. |
| 3DS.data.tag | MAC of encryptedMessage |

The request should include the following HTTP headers:</p>

| Header | Description |
| ------ | ----------- |
| apikey | The API Key provided by First Data |
| token | The token provided by First Data |
| content-type | "Application/json" |
| Authorization | Value computed from the Secret and the payload (see the code to compute the HMAC in the Pay with Google sample application) |
| nonce | see the code to compute the HMAC in the Pay with Google sample application |
| timestamp | see the code to compute the HMAC in the Pay with Google sample application |

The response from the First Data servers describes the results of the transaction. A sample response:</p>

```
{
  "correlation_id": "55.5102426711995",
  "transaction_status": "approved",
  "validation_status": "success",
  "transaction_type": "purchase",
  "transaction_id": "097194",
  "transaction_tag": "2200452661",
  "method": "3ds",
  "amount": "1",
  "currency": "USD",
  "token": {
    "token_type": "FDToken",
    "token_data": {
      "value": "8547304243291111"
    }
  },
  "card": {
    "type": "VISA",
    "cardholder_name": "Not Provided",
    "card_number": "1111",
    "exp_date": "1222"
  },
  "bank_resp_code": "100",
  "bank_message": "Approved",
  "gateway_resp_code": "00",
  "gateway_message": "Transaction Normal"
}
``` 

For an explanation of the response fields please refer to the [Pay with Google API](https://developer.payeezy.com/payeezy-api/apis/post/transactions-17) at the Developer Portal.</p>

# Sample Application 
The <a href="https://github.com/payeezy/pay_with_google/tree/master/sdk">sample application</a> provided on GitHub demonstrates how First Data's RESTful API can be used to process Pay with Google requests. The application makes use of the <a href="http://developer.android.com/training/volley/index.html" rel="nofollow">Volley library</a> to issue REST requests to the First Data servers.

The application requires the following permissions:

- android.permission.INTERNET
- android.permission.ACCESS_NETWORK_STATE





  
