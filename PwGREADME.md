# Pay With Google
The document describes how developer can use First Data's RESTful API solution to process a Pay with Google  transaction.
The integration process first step consists of registering the developer and boarding the merchant into the API platform, and then getting the required credentials.

Refer to the <a href="https://github.com/payeezy/get_started_with_payeezy/blob/master/get_started_with_payeezy042015.pdf">Getting Started Guide</a> for instructions how to register and get the information to get you going.

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
For a full explanation of the API please refer to the <a href="https://developer.payeezy.com/" rel="nofollow">Developer Portal</a>.
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
_______
</tr>
</thead>
<tbody>
<tr>
<td>API Key</td>
<td>HTTP header</td>
</tr>
<tr>
<td>Token</td>
<td>HTTP header</td>
</tr>
<tr>
<td>API Secret</td>
<td>Used to compute the HMAC. The HMAC is added to the request through the HTTP headers.</td>
</tr></tbody></table>
<h2><a href="#sample-request-payload" aria-hidden="true" class="anchor" id="user-content-sample-request-payload"><svg aria-hidden="true" class="octicon octicon-link" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Sample Request Payload</h2>
<p>The following is an example of a FirstAPI request payload:</p>
<pre><code>{  "currency_code": "USD",  
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
</code></pre>
<p>The following table describes the contents of the request fields:</p>
<table>
<thead>
<tr>
<th>Field</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>currency_code</td>
<td>Code of the currency used in the transaction. For example, USD denotes US Dollars.</td>
</tr>
<tr>
<td>amount</td>
<td>The transaction amount in cents. For example, 10000 is $100.00.</td>
</tr>
<tr>
<td>merchant_ref</td>
<td>The merchant's order number.</td>
</tr>
<tr>
<td>transaction_type</td>
<td>The transaction type the merchant wants to process</td>
</tr>
<tr>
<td>method</td>
<td>Must be "3DS"</td>
</tr>
<tr>
<td>transaction_type</td>
<td>Describes the type of transaction, for example "purchase"</td>
</tr>
<tr>
<td>3DS.signature</td>
<td>Signature for verifying that the message comes from Google</td>
</tr>
<tr>
<td>3.DS.type</td>
<td>Should be set to "G" for Pay with Google</td>
</tr>
<tr>
<td>3DS.version</td>
<td>Should be set to ECv1</td>
</tr>
<tr>
<td>3DS.data.encryptedMessage</td>
<td></td>
</tr>
<tr>
<td>3DS.data.ephemeralPublicKey</td>
<td>The "ephemeralPublicKey" field extracted from the token returned by Pay with Google.</td>
</tr>
<tr>
<td>3DS.data.tag</td>
<td>MAC of encryptedMessage</td>
</tr></tbody></table>
<p>The request should include the following HTTP headers:</p>
<table>
<thead>
<tr>
<th>Header</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>apikey</td>
<td>The API Key provided by First Data</td>
</tr>
<tr>
<td>token</td>
<td>The token provided by First Data</td>
</tr>
<tr>
<td>content-type</td>
<td>"Application/json"</td>
</tr>
<tr>
<td>Authorization</td>
<td>Value computed from the Secret and the payload (see the code to compute the HMAC in the Pay with Google sample application)</td>
</tr>
<tr>
<td>nonce</td>
<td>see the code to compute the HMAC in the Pay with Google sample application</td>
</tr>
<tr>
<td>timestamp</td>
<td>see the code to compute the HMAC in the Pay with Google sample application</td>
</tr></tbody></table>
<p>The response from the First Data servers describes the results of the transaction. A sample response:</p>
<pre><code>{
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
</code></pre>
<p>For an explanation of the response fields please refer to the <a href="https://developer.payeezy.com/payeezy-api/apis/post/transactions-17" rel="nofollow">Pay with Google API</a> at the Developer Portal.</p>
<h1><a href="#sample-application" aria-hidden="true" class="anchor" id="user-content-sample-application"><svg aria-hidden="true" class="octicon octicon-link" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Sample Application </h1>
<p>The <a href="https://github.com/payeezy/pay_with_google/tree/master/sdk">sample application</a> provided on GitHub demonstrates how First Data's RESTful API can be used to process Pay with Google requests. The application makes use of the <a href="http://developer.android.com/training/volley/index.html" rel="nofollow">Volley library</a> to issue REST requests to the First Data servers.</p>
<p>The application requires the following permissions:</p>
<ul>
<li>
<p>android.permission.INTERNET</p>
</li>
<li>
<p>android.permission.ACCESS_NETWORK_STATE
 </p>
</li>
</ul>
</article>
  </div>

  
