# FirstAPI Pay With Google

The document describes how developer can use FirstAPI to process a Pay with Google  transaction.
The integration process first step consists of boarding the developer and the merchant into the FirstAPI platform, getting the required credentials.
Once boarded, the application can be implemented and tested by sending requests to the FirstAPI platforms, using the credentials received in the first step.
# Requirements
- Android version 4.4 (KitKat) or higher
- The Pay with Google application installed on the test device
- The latest version of Google Play Services (11.4.x)

# Obtaining Credentials
The following information is provided when you register on the FirstAPI Developer Portal:
- API Key – identifies the Developer
- Token – identifies the Merchant
- API Secret – used to compute the HMAC signature

Refer to the [Getting Started Guide](https://github.com/payeezy/get_started_with_payeezy/blob/master/get_started_with_payeezy042015.pdf) for instructions how to register with FirstAPI and get the information to get you going.


# Application Flow
The typical flow of an application using Pay with Google will be as following:

 1. Interacting with the user to get the service or merchandise that the user wishes to purchase. 
 
 2. Displaying the "Pay with Google" button.
 
 3. Sending a credential request to Pay with Google with the Processor name and Merchant ID.
 
 4. Obtaining the Payment Credentials encrypted with the First Data key from Google.
 
 5. Using the Payment Credentials to send the request to the FirstAPI server.
 
 6. Process the request results

The following sections describe in more detail how to use the FirstAPI credentials to interact with Pay with Google and with FirstAPI.

## Requesting Credentials

To create the credential request the developer needs:
1. The Merchant ID
2. The Gateway Tokenization parameter, which is set to 'firstdata'

Google will return the encrypted payload to the app. 

## Issuing the FirstAPI Request

The FirstAPI request uses the RESTful library Volley to send the request to First Data (the payment processor.) As mentioned before, the remaining items issued by FirstAPI will be used when issuing the request. The following table describes where the items are used:

| Name | Used in |
| --- | --- |
| API Key | HTTP header |
| Token | HTTP header |
| API Secret | Used to compute the HMAC. The HMAC is added to the request through the HTTP headers. |

## The FirstAPI Request
For a full explanation of the FirstAPI API please refer to the FirstAPI [Developer Portal] (https://developer.payeezy.com/). 

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
| --- | --- |
| currency\_code | Code of the currency used in the transaction. For example, USD denotes US Dollars. |
| amount | The transaction amount in cents. For example, 10000 is $100.00. |
| merchant\_ref | The merchant's order number. |
| transaction\_type | The transaction type the merchant wants to process
| method | Must be "3DS" |
| transaction\_type | Describes the type of transaction, for example "purchase" |
| 3DS.signature | Signature for verifying that the message comes from Google |
| 3.DS.type | Should be set to "G" for Pay with Google |
| 3DS.version | Should be set to ECv1 |
| 3DS.data.encryptedMessage |  |The encrypted message containing the actual payment information as well as additional security fields. |
| 3DS.data.ephemeralPublicKey | The "ephemeralPublicKey" field extracted from the token returned by Pay with Google. |
| 3DS.data.tag | MAC of encryptedMessage |


The request should include the following HTTP headers:

| Header | Description |
| --- | --- |
| apikey | The API Key provided by FirstAPI |
| token | The token provided by FirstAPI |
| content-type | "Application/json" |
| Authorization | Value computed from the Secret and the payload (see the code to compute the HMAC in the Pay with Google sample application) |
| nonce | see the code to compute the HMAC in the Pay with Google sample application |
| timestamp | see the code to compute the HMAC in the Pay with Google sample application |

The response from the FirstAPI servers describes the results of the transaction. A sample response:
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
For an explanation of the response fields please refer to the FirstAPI Developer Portal at [https://developer.payeezy.com/payeezy-api/apis/post/transactions](https://developer.payeezy.com/payeezy-api/apis/post/transactions).

# Sample Application 
The sample application provided on GitHub demonstrates how FirstAPI can be used to process Pay with Google requests. The application makes use of the [Volley library] (http://developer.android.com/training/volley/index.html) to issue REST requests to the FirstAPI servers.

The application requires the following permissions:

- android.permission.INTERNET

- android.permission.ACCESS_NETWORK_STATE
 
