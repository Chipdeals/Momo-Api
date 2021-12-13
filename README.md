<!-- 
-request payment
-register
-get balance
-deposit
-get history 
-get transaction for a phone number
-split payment
-login by Momo
-multi currency supported

-->



# Chipdeals Momo api Documentation

## Welcome to the Chipdeals Momo API documentation where you will learn how to set up an excellent payment experience with the Chipdeals Momo API

## **You can [request payment](#request-payment) and [send money](#deposit) to any mobile money account**

## 
<details>
  <summary> Error codes </summary>

  **400-000** 👉 Endpoint not available <br/>
  **400-100** 👉 Parameter senderFirstName not found in the request <br/>
  **400-101** 👉 Parameter senderFirstName contain invalid data <br/>
  **400-102** 👉 Parameter senderLastName not found in the request <br/>
  **400-103** 👉 Parameter senderLastName contain invalid data <br/>
  **400-104** 👉 Parameter senderPhoneNumber not found in the request <br/>
  **400-105** 👉 Parameter senderPhoneNumber contain invalid data <br/>
  **400-106** 👉 Parameter amount not found in the request <br/>
  **400-107** 👉 Parameter amount contain invalid data <br/>
  **400-108** 👉 Parameter businessName not found in the request <br/>
  **400-109** 👉 Parameter businessName contain invalid data <br/>
  **400-110** 👉 Parameter recipientPhoneNumber not found in the request <br/>
  **400-111** 👉 Parameter recipientPhoneNumber contain invalid data <br/>
  **401-100** 👉 Incorrect api key in the request <br/>
  **404-100** 👉 No transaction found with the reference in the request <br/>
  **429-000** 👉 Rate limit exceeded for test mode. 10 request par minute are allowed <br/>
  **500-000** 👉 Sorry an error occured on the server. try again. <br/>

</details>

[Important Note](#important)

## Uses cases

### Request payment

**Send payment ussd push to a client to ask his money.**

`POST` request to `https://apis.chipdeals.me/momo/requestpayment?apikey=<YOUR_API_KEY>`

You can get you apiKey [here](#register)
  
<u> Request: </u>

```http
POST https://apis.chipdeals.me/momo/requestpayment?apikey=efd2db6c-7a43-408a-880f-a39ed56f0eff HTTP/1.1
content-type: application/json

{
  "senderFirstName": "Euler Géraud", //User first name 
  "senderLastName": "Dougbe", //User last name 
  "senderPhoneNumber": "22951010580", //Phone number with country prefix. Default 229 is used as country prefix if no country prefix found
  "amount": 5000, //money requested
}
```
<u> Responses: </u>
  
<details>
  <summary>Success ➛ Status code : 200 - OK </summary>

  <br/>

  > Description: Money successfuly requested

  response:
  ```json
    {
      "success": true, //Success status of the request
      "message": "", //Message of the request status
      "payment": {
        "reference": "dd1e2d17-5c21-4964-b58d-198fd2aac150", // reference of the transaction Chipdeas Momo Api
        "senderPhoneNumber": "22951010580",
        "senderOperator": "MTN",
        "senderFirstName": "Euler",
        "senderLastName": "Dougbe",
        "currency": "xof", //currency of the transaction. only xof for the moment
        "amount": 5000, //amount of the transaction
        "status": "pending", //simple status of the transaction. pending or success or error
        "statusMessage": "pending", // Text explaining the state of the transaction
        "statusMessageCode": 202, // Code of the state of the transaction. 202 means pending. 200 means success.
        "startTimestampInSecond": 1638087167, // Timestamp in second of whe you init the transaction
        "endTimestampInSecond": 0 //Timestamp in second of when the transaction is finished
      }
    }

  ```
  Get all possible statusMessageCode [here](#status-message-code)
</details>

<details>
  <summary>Error ➛ Status code : 400 - BAD REQUEST </summary>
  
  <br/>

  > Description: All the required parameters are not entered in the request
  
  response:
  ```json
    {
      "success": false,
      "message": "Parameter senderPhoneNumber not found in the request",
      "errorCode":"400-104", //Code of the response
      "payment":{}
    }

  ```
</details>

<details>
  <summary>Error ➛ Status code : 401 - UNAUTHORIZED </summary>
  
  <br/>

  > Description: Incorrect api key 
  
  response:
  ```json
    {
      "success": false,
      "message": "Incorrect api key in the request",
      "errorCode":"401-100",
      "payment":{}
    }

  ```
</details>

You can check status of the request [here](#get-status)

---
<br/>


### Register 

**Register your application an gat an api key for operations in Chipdeals Momo Api**

`POST` request to `https://apis.chipdeals.me/momo/register`
  
<u> Request: </u>

```http
POST https://apis.chipdeals.me/momo/register HTTP/1.1
content-type: application/json
userUuid: 31ac56c3-4ed1-42d8-94a6-b9c3e6a6be57 //User uuid got after login or sign up

{
  "businessName": "Amazing payment app", //Your business name
}
```

<u> Responses: </u>
  
<details>
  <summary>Success ➛ Status code : 201 - CREATED </summary>

  <br/>

  > Description: You are successfully registered in the system

  response:
  ```json
    {
      "success": true,
      "message": "",
      "apikey": "efd2db6c-7a43-408a-880f-a39ed56f0eff"
    }

  ```
</details>

<details>
  <summary>Error ➛ Status code : 400 - BAD REQUEST </summary>
  
  <br/>

  > Description: All the required parameters are not entered in the request
  
  response:
  ```json
    {
      "success": false,
      "message": "Parameter businessName not found in the request",
      "errorCode":"400-108",
      "apikey":""
    }

  ```
</details>

---
<br/>

### Get Status 

**Check the state of the transactions you launched**

`GET` request to `https://apis.chipdeals.me/momo/status/<TRANACTION_REFERENCE>?apikey=<YOUR_API_KEY>`

The `<TRANACTION_REFERENCE>` is the field `reference` of your payment request or deposit response
  
<u> Request: </u>

```http
GET https://apis.chipdeals.me/momo/status/dd1e2d17-5c21-4964-b58d-198fd2aac150?apikey=efd2db6c-7a43-408a-880f-a39ed56f0eff HTTP/1.1

```

<u> Responses: </u>
  
<details>
  <summary>Success ➛ Status code : 200 - OK </summary>

  <br/>

  > Description: Transaction found

  response:
  ```json
    {
      "success": true,
      "message": "",
      "transaction": {
        "reference": "dd1e2d17-5c21-4964-b58d-198fd2aac150", // reference of the transaction Chipdeas Momo Api
        "senderPhoneNumber": "22951010580",
        "senderOperator": "MTN",
        "senderFirstName": "Euler",
        "senderLastName": "Dougbe",
        "currency": "xof", //currency of the transaction. only xof for the moment
        "amount": 5000, //amount of the transaction
        "status": "success", //simple status of the transaction. pending or success or error
        "statusMessage": "transaction successfully processed", // Text explaining the state of the transaction
        "statusMessageCode": 200, // Code of the state of the transaction. 202 means pending. 200 means success.
        "startTimestampInSecond": 1638087167, // Timestamp in second of whe you init the transaction
        "endTimestampInSecond": 0 //Timestamp in second of when the transaction is finished
      }
    }

  ```

  Get all possible statusMessageCode [here](#status-message-code)
</details>

<details>
  <summary>Error ➛ Status code : 401 - UNAUTHORIZED </summary>
  
  <br/>

  > Description: Incorrect api key 
  
  response:
  ```json
    {
      "success": false,
      "message": "Incorrect api key in the request",
      "errorCode":"401-100",
      "transaction":{}
    }

  ```
</details>

<details>
  <summary>Error ➛ Status code : 404 - NOT FOUND </summary>
  
  <br/>

  > Description: No transaction found with the reference in the request
  
  response:
  ```json
    {
      "success": false,
      "message": "No transaction found with the reference in the request",
      "errorCode":"404-100",
      "transaction":""
    }

  ```
</details>

Check your balance [here](#balance)

Get all possible statusMessageCode [here](#status-message-code)

---
<br/>



### Balance 

**Get your balance in Chipdeals Momo Api**

`GET` request to `https://apis.chipdeals.me/momo/balance?apikey=<YOUR_API_KEY>`

You can get you apiKey [here](#register)
  
<u> Request: </u>

```http
GET https://apis.chipdeals.me/momo/balance?apikey=efd2db6c-7a43-408a-880f-a39ed56f0eff HTTP/1.1

```

<u> Responses: </u>
  
<details>
  <summary>Success ➛ Status code : 200 - OK </summary>

  <br/>

  > Description: Balance got

  response:
  ```json
    {
      "success": true,
      "message": "",
      "balance": {
        "currentBalance": 13100, // Amount currently in your account
        "totalDepositAmount": 100000, //total amount for successfully processed deposit
        "totalPaymentAmount": 85000, //total amount for successfully processed payment request
        "currency": "xof" 
      }
    }

  ```

</details>

<details>
  <summary>Error ➛ Status code : 401 - UNAUTHORIZED </summary>
  
  <br/>

  > Description: Incorrect api key 
  
  response:
  ```json
    {
      "success": false,
      "message": "Incorrect api key in the request",
      "errorCode":"401-100",
      "transaction":{}
    }

  ```
</details>

Transfer money from your account to any mobile account [here](#deposit)

---
<br/>




### Deposit

**Transfer money from your account to any mobile account**

`POST` request to `https://apis.chipdeals.me/momo/deposit?apikey=<YOUR_API_KEY>`

You can get you apiKey [here](#register)
  
<u> Request: </u>

```http
POST https://apis.chipdeals.me/momo/deposit?apikey=efd2db6c-7a43-408a-880f-a39ed56f0eff HTTP/1.1
content-type: application/json

{
  "recipientPhoneNumber": "22951010580", //Phone number with country prefix. Default 229 is used as country prefix if no country prefix found
  "amount": 5000, //money requested
}
```
<u> Responses: </u>
  
<details>
  <summary>Success ➛ Status code : 200 - OK </summary>

  <br/>

  > Description: Deposit successfully started

  response:
  ```json
    {
      "success": true, //Success status of the request
      "message": "", //Message of the request status
      "deposit": {
        "reference": "dd1e2d17-5c21-4964-b58d-198fd2aac150", // reference of the transaction Chipdeas Momo Api
        "recipientPhoneNumber": "22951010580",
        "recipientOperator": "MTN",
        "currency": "xof", //currency of the transaction. only xof for the moment
        "amount": 5000, //amount of the transaction
        "status": "pending", //simple status of the transaction. pending or success or error
        "statusMessage": "pending", // Text explaining the state of the transaction
        "statusMessageCode": 202, // Code of the state of the transaction. 202 means pending. 200 means success.
        "startTimestampInSecond": 1638087167, // Timestamp in second of whe you init the transaction
        "endTimestampInSecond": 0 //Timestamp in second of when the transaction is finished
      }
    }

  ```
  Get all possible statusMessageCode [here](#status-message-code)
</details>

<details>
  <summary>Error ➛ Status code : 400 - BAD REQUEST </summary>
  
  <br/>

  > Description: All the required parameters are not entered in the request
  
  response:
  ```json
    {
      "success": false,
      "message": "Parameter recipientPhoneNumber not found in the request",
      "errorCode":"400-110",
      "deposit":{}
    }

  ```
</details>

<details>
  <summary>Error ➛ Status code : 401 - UNAUTHORIZED </summary>
  
  <br/>

  > Description: Incorrect api key 
  
  response:
  ```json
    {
      "success": false,
      "message": "Incorrect api key in the request",
      "errorCode":"401-100",
      "deposit":{}
    }

  ```
</details>

You can check status of the deposit [here](#get-status)

---
<br/>


### Get history

**Get history of all transactions ordered by date from the last to the first**

`GET` request to `https://apis.chipdeals.me/momo/history?apikey=<YOUR_API_KEY>`

You can get you apiKey [here](#register)
  
<u> Request: </u>

```http
GET https://apis.chipdeals.me/momo/history?apikey=efd2db6c-7a43-408a-880f-a39ed56f0eff HTTP/1.1
content-type: application/json
```
<u> Responses: </u>
  
<details>
  <summary>Success ➛ Status code : 200 - OK </summary>

  <br/>

  > Description: Success

  response:
  ```json
    {
      "success": true, //Success status of the request
      "message": "", //Message of the request status
      "payments": [
        {
          "reference": "dd1e2d17-5c21-4964-b58d-198fd2aac150", 
          "senderPhoneNumber": "22951010580",
          "senderOperator": "MTN",
          "senderFirstName": "Euler",
          "senderLastName": "Dougbe",
          "currency": "xof", //currency of the transaction. only xof for the moment
          "amount": 5000, //amount of the transaction
          "status": "success", //simple status of the transaction. pending or success or error
          "statusMessage": "transaction successfully processed", // Text explaining the state of the transaction
          "statusMessageCode": 200, // Code of the state of the transaction. 202 means pending. 200 means success.
          "startTimestampInSecond": 1638087167, // Timestamp in second of whe you init the transaction
          "endTimestampInSecond": 0 //Timestamp in second of when the transaction is finished
        }
      ],
      "deposits": [
        {
          "reference": "dd1e2d17-5c21-4964-b58d-198fd2aac150", // reference of the transaction Chipdeas Momo Api
          "recipientPhoneNumber": "22951010580",
          "recipientOperator": "MTN",
          "currency": "xof", //currency of the transaction. only xof for the moment
          "amount": 5000, //amount of the transaction
          "status": "pending", //simple status of the transaction. pending or success or error
          "statusMessage": "pending", // Text explaining the state of the transaction
          "statusMessageCode": 202, // Code of the state of the transaction. 202 means pending. 200 means success.
          "startTimestampInSecond": 1639087167, // Timestamp in second of whe you init the transaction
          "endTimestampInSecond": 0 //Timestamp in second of when the transaction is finished
        }
      ],
      "allTransactions": [
        {
          "reference": "dd1e2d17-5c21-4964-b58d-198fd2aac150", 
          "recipientPhoneNumber": "22951010580",
          "recipientOperator": "MTN",
          "currency": "xof", 
          "amount": 5000, 
          "status": "pending", 
          "statusMessage": "pending", 
          "statusMessageCode": 202, 
          "startTimestampInSecond": 1639087167,
          "endTimestampInSecond": 0 
        },
        {
          "reference": "dd1e2d17-5c21-4964-b58d-198fd2aac150", 
          "senderPhoneNumber": "22951010580",
          "senderOperator": "MTN",
          "senderFirstName": "Euler",
          "senderLastName": "Dougbe",
          "currency": "xof", 
          "amount": 5000, 
          "status": "success", 
          "statusMessage": "transaction successfully processed", 
          "statusMessageCode": 200, 
          "startTimestampInSecond": 1638087167,
          "endTimestampInSecond": 0 
        }

      ]
    }

  ```
  Get all possible statusMessageCode [here](#status-message-code)
</details>

<details>
  <summary>Error ➛ Status code : 401 - UNAUTHORIZED </summary>
  
  <br/>

  > Description: Incorrect api key 
  
  response:
  ```json
    {
      "success": false,
      "message": "Incorrect api key in the request",
      "errorCode":"401-100",
      "deposit":{}
    }

  ```
</details>

You can check status of the deposit [here](#get-status)

---
<br/>


##
### Status Message Code
**Meaning of status message code**

  **200 👉 ✔️ Transaction successful** <br/>
  **201 👉 🕟 Data in validation** <br/>
  **202 👉 🕟 Transaction pending** <br/>
  **203 👉 🕟 Data are validated, server is working** <br/>
  **204 👉 🕟 Waiting for ussd push validation** <br/>
  **400 👉 ❌ Incorrect data enter in the request** <br/>
  **401 👉 ❌ Parameters not complete** <br/>
  **402 👉 ❌ Payment PhoneNumber is not correct** <br/>
  **403 👉 ❌ Deposit PhoneNumber is not correct** <br/>
  **404 👉 ❌ Timeout in USSD PUSH/ Cancel in USSD PUSH** <br/>
  **460 👉 ❌ Payer's payment account balance is low** <br/>
  **461 👉 ❌ An error occured while paying** <br/>
  **462 👉 ❌ This kind of transaction is not supported yet, processor not found** <br/>
  **500 👉 ⛔️☠️ An unknow error occured on the api** <br/>
  **500 👉 ⛔️☠️ Transaction not Exists. Server error with payment service** <br/>
  **500 👉 ⛔️☠️ An undefined error occured** <br/>
  **501 👉 ⛔️☠️🔔 A several error occured on the api. Call The dev Please** <br/>
  **504 👉 ⛔️☠️ Deposit is not available for the moment, try again later** <br/>
  <!-- **230 👉 🕟 Payment successfull, deposit pending** <br/> -->
  <!-- **502 👉 ⛔️☠️ No Available Amount On the server To Withdraw** <br/> -->
  <!-- **503 👉 ⛔️☠️ An error occured while deposit is processing after payment is successful** <br/> -->

### IMPORTANT
**There is some limitations when you are using your test api key and you should not ignore them**

- Using test apikey have a rate limit of 10 requests per minute
- With test apikey, the max amount you can request id 1. Any value over than 1 will be replaced by 1
    
    

##
### Copyright
```
   Copyright (C) 2021 Chipdeals Inc

```

