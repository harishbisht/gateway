# Welcome to zookr payments gateway



Need a payment solution for your website? We’ll have you up and be running in no time!

You are only two steps away from accepting payments with zookr gateway
**Contact at:**  help@zookr.in



# API
```
URL: https://zookr.in
```
 ## Transaction Request API (POST)
 ```
 /api/gateway/live/payment/
 ```
| Parameter Name	 | Description |
| --- | --- |
| merchant_id | This is a unique merchant ID provided to the merchant by zookr at the time of merchant creation. |
| customer_email_id | The “Customer Email id” is the customer identifier. This could be a unique user Id that the Merchant has assigned to its customers. |
| txn_amount | This is the “Transaction Amount” that is to be charged to the customer’s credit card /debit card /bank account. Please ensure that the amount is in the same currency as defined for the Merchant ID being used. |
| order_id | The “Order ID” is the Merchant’s Transaction ID which should be unique for every transaction. Duplicate order id will be rejected by the zookr gateway. You may use UNIX time stamp appended with a random string to ensure that a duplicate Order Id is never passed. |
| callback_url | Merchant will get a response to this URL if this parameter is sent in the transaction request. This call back URL will get priority over the static call back URL configured during the time of integration. |
| checksumhash | URL encoded Checksum to be calculated based on a pre defined logic as given below in the “Generating Checksum” section. Checksum is used to ensure data has not tampered when a request is posted on the Zookr URL. |


> **Note:** Once the response is received, the merchant must verify the Checksumhash received against Checksumhash created using the Server Side utilities provided by zookr. Verifying the Checksumhash ensures that the response has not been tampered with.
> **Example of tampering: Failed transaction marked as successful!**

> **Note:** txn_amount should be in floating point



## Transaction Status API (GET)
As per secure practices, before marking the transaction as success in the system on receiving success response from zookr, the merchant should re-verify transaction status and amount of order by calling zookr status query API from their back-end server.

zookr status query API returns the ORDERID, TXNAMOUNT, STATUS and some additional parameters for a given order.

If ORDERID and TXNAMOUNT parameters do not match with the order data on merchant side or STATUS is not TXN_SUCCESS, the merchant should not fulfill/deliver the order.

The other scenarios where status query will come handy are as follows –

zookr returns pending status in response. It happens when the bank does not provide the final response to zookr.
Due to network drop or Internet issue, the merchant does not get the response from zookr.
```
 /api/gateway/live/status/
```
| Parameter Name	 | Description |
| --- | --- |
| merchant_id | This is a unique merchant ID provided to the merchant by zookr at the time of merchant creation. |
| order_id | This is the application transaction ID that was sent by the merchant to zookr at the time of transaction request. |
| checksumhash | URL encoded checksum which is calculated based on a predefined logic |




# FAQ
> **How to generate the checksumhash?**
Find the security file for your language and save the file to your server, now pass all the parameters to the function **getChecksum** available in security file, this function will return you the checksumhash, now do the [URL encoded](https://en.wikipedia.org/wiki/Percent-encoding) and add the checksumhash in your GET request

 
> **What is the process flow of payment gateway?**
call the payment gateway URL with all parameters, after that, we will redirect you to the payment page, after making the payment we will send one POST request to your callback_url with response data and then we will redirect you to your callback_url

>**How do we verify the callback_url authentication?**
We will send the checksumhash while making the callback request and you can verify the checksum using the **verifychecksum** function available in security file

>**Give one example for generating the checksumhash**
>```
>parameters => {'merchant_id'='fl443mfmkla95645', 
>'customer_email_id'='email@email.com', 
>'txn_amount'='10.0', 
>'order_id'='QM122',
>'callback_url'='https://www.mywebsite.com/paymentgatewatresponse'}
>```
>Now pass all the parameters to the function **getChecksum** available in security file, this function will return you the checksumhash, now do the [URL encoded](https://en.wikipedia.org/wiki/Percent-encoding) and add the checksumhash in your GET request
>Example: getChecksum(parameters,'**YOUR_16_DIGIT_MERCHANT KEY**')
>Now add the checksumhash in your parameter,
>```
>parameters => {'merchant_id'='fl443mfmkla95645',
> 'customer_email_id'='email@email.com', 
> 'txn_amount'='10.0',
> 'order_id'='QM122',
>'callback_url'='https://www.mywebsite.com/paymentgatewatresponse'
>'checksumhash'='YOUR_GENERATE_CHECKSUMHASH'}
>```
> Now make a request like this: 
> Example of url:
> ```
> https://zookr.in/api/gateway/live/payment/?merchant_id=fl443mfmkla95645&customer_email_id=email@email.com&txn_amount=10.0&order_id=QM122&callback_url=https://www.mywebsite.com/paymentgatewatresponse&checksumhash=YOUR_GENERATE_CHECKSUMHASH
> ```

If you have any query you can reach me at harish@pickrr.com
