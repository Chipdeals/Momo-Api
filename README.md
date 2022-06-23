# Mobile Money API for [REST Http](https://github.com/Chipdeals/Momo-Api), and [Nodejs](https://github.com/Chipdeals/Momo-Api/blob/main/node-js.md)

![Nodejs](https://img.shields.io/badge/rest_http-43853D?style=for-the-badge&logo=web&logoColor=white)

**Chipdeals-momo-api** is a Mobile Money API that allows you to build a quick, simple and excellent payment experience in your web and native app. This is the official **Doc for with http requests**


**You can [request payment](#Collect-Money) and [send money](#Disburse-Money) to any mobile money account**

# Requirements

No requirement

<br/>

# Installation

You can use any tools that allow you to perform http requests.
In this documentation, I will use curl, so I hope you have your terminal or another test tools to fast test all. (me i use [VSCode REST Client extention](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) to check all quickly)

<br/>

# Quick Start

**Make your first request with your API Key ([*Get apikey here*](#Contact-us))**

#### Collection
```sh
curl --request POST 'https://apis.chipdeals.me/momo/requestpayment?apikey=test_FOdigzgSopV8GZggZa89' \
--header 'Content-Type: application/json' \
--data-raw '{
  "senderFirstName": "Jean",
  "senderLastName": "EVERICH",
  "senderPhoneNumber": "22951945229",
  "amount": 10,
  "currency": "XOF"
}'
```

#### Disbursement
```sh
curl --request POST 'https://apis.chipdeals.me/momo/deposit?apikey=test_FOdigzgSopV8GZggZa89' \
--header 'Content-Type: application/json' \
--data-raw '{
  "recipientPhoneNumber": "22952038945",
  "amount": 10,
  "currency": "XOF"
}'
```

###### Quick check possible responses with [sandbox tests](#Sandbox-tests)

<br/>

# Usage

#### Authentication
You need to specify an apikey in all call you make to our API.

You specify you apikey with the query parametter ``apikey`` like in the folowing sample
```sh
curl 'https://chipdealsApiUrl?apikey=YOUR_API_KEY_HERE'
```


[***You can get your apiKey here***](#Contact-us)

<br/>

## Collect Money


### Simple collection
For example to request 2000 XOF from the ***+22951010591*** Mobile Money wallet, the following code can be used

```sh
curl --request POST 'https://apis.chipdeals.me/momo/requestpayment?apikey=test_FOdigzgSopV8GZggZa89' \
--header 'Content-Type: application/json' \
--data-raw '{
  "senderFirstName": "Jean",
  "senderLastName": "EVERICH",
  "senderPhoneNumber": "22951945229",
  "amount": 2000,
  "currency": "XOF"
}'
```

[Here are collection model of response you will get after collection request](#Each-Responses-model)

### Collect with a [webhook](#Webhook) to get response as soon as the payment is processed. 

```sh
curl --request POST 'https://apis.chipdeals.me/momo/requestpayment?apikey=test_FOdigzgSopV8GZggZa89' \
--header 'Content-Type: application/json' \
--data-raw '{
  "senderFirstName": "Jean",
  "senderLastName": "EVERICH",
  "senderPhoneNumber": "22951945229",
  "amount": 2000,
  "currency": "XOF",
  "webhookUrl": "https://mydomain/collection-status"
}'
```

[*See webhook you get*](#Collectiton-state-changed-webhook-payload-sample)

## Disburse Money

### Simple Disbursement

You can also send 2000 XOF to the ***+22951010591*** Mobile Money wallet, with the following code



```sh
curl --request POST 'https://apis.chipdeals.me/momo/deposit?apikey=test_FOdigzgSopV8GZggZa89' \
--header 'Content-Type: application/json' \
--data-raw '{
  "recipientPhoneNumber": "22952038945",
  "amount": 2000,
  "currency": "XOF"
}'
```

[Here are disbursement model of response you will get after disbursement request](#Each-Responses-model)

### Disburse with a [webhook](#Webhook) to get response as soon as the deposit is processed. 

```sh
curl --request POST 'https://apis.chipdeals.me/momo/deposit?apikey=test_FOdigzgSopV8GZggZa89' \
--header 'Content-Type: application/json' \
--data-raw '{
  "recipientPhoneNumber": "22952038945",
  "amount": 2000,
  "currency": "XOF",
  "webhookUrl": "https://mydomain/collection-status"
}'
```
[*See webhook you get*](#Disbursement-state-changed-webhook-payload-sample)


## Get transaction status

Get status of a transaction of reference `dd1e2d17-5c21-4964-b58d-198fd2aac150`



```sh
curl --request GET 'https://apis.chipdeals.me/momo/status/39022f7a-abd1-468f-a682-a6e2024d65b0?apikey=test_FOdigzgSopV8GZggZa89'
``` 
[Transaction respnoses sample](#Each-Responses-model)


<br/>

## Get Your balance

Get your Chipdeals account's balance

```sh
curl --request GET 'https://apis.chipdeals.me/momo/balance?apikey=test_FOdigzgSopV8GZggZa89'

```

Balance sample:

```json
{
  "success": true,
  "message": "",
  "balance": [
    {
      "amount": 2000,
      "currency": "XOF"
    },
    {
      "amount": 1045,
      "currency": "XAF"
    },
  ]
}
```

<br/>
<br/>

# Responses

### All responses model

```json
  {
    "success": true, //REQUIRED. Status of the request processing. If true, that means there is no probleme for the request
    "message": "", //REQUIRED. Processing message. If there is no text that means that there where no probleme for the request. If it contain a text, it specify the state of the request.
    "errorCode": "code" //OPTIONAL. This parametter is added only when there is an error in the request. Is is so added only when success parameter contain false. It contain a specific code relative to the error that occured.
    // Other parametters specific to the request
    
  }
  ```



### Each Responses model:

<details>
  <summary>Collection  </summary>
  
  #### Collection Responses model

  ```json
  {
    "success": true, //status of the request processing
    "message": "", //message of the request processing
    "payment": {
      "reference": "b8f9aeb4-ca07-4438-b9ce-aa975a39d15d", //reference of the transaction in our system
      "senderPhoneNumber": "22951945229", //Phone number of the money sender
      "senderCountryCode": "BJ", //Country code of the money sender
      "senderOperator": "MTN", //Operator of the money sender
      "senderFirstName": "Jean", //Firstname of the money sender
      "senderLastName": "EVERICH", //Lastname of the money sender
      "originalCurrency": "XOF", //Currency of payment you specified in your request
      "currency": "XOF", //Local currency of the sender phone number
      "status": "pending", //Status of the transaction (pending || success || error)
      "statusMessage": "pending", //Message explaining what is exactly hapennig for the transaction
      "statusMessageCode": 202, //Specific code of the exact state of the transaction
      "startTimestampInSecond": 1655968250, //Timestamp in second of the moment when you initiated the transaction
      "endTimestampInSecond": 0, //Timestamp in second of the moment when a tranaction is finished
      "amount": 10, //Amount in local currency that the sender will pay
      "originalAmount": 10 //Amount you specified the user should pay in your request
    }
  }
  ```
</details>

<details>
  <summary>Disbursement  </summary>
  
  #### Disbursement Responses model

  ```json
  {
    "success": true, //status of the request processing
    "message": "", //message of the request processing
    "deposit": {
      "reference": 9d621c8f-5a8e-4f88-ad24-984fb47a8ce8//reference of the transaction in our system
      "recipientPhoneNumber": "22951945229", //Phone number of the money recipient
      "recipientOperator": "MTN", //Operator of the money recipient
      "recipientCountryCode": "BJ", //Country code of the money recipient
      "originalCurrency": "XOF", //Currency of deposit you specified in your request
      "currency": "XOF", //Local currency of the sender phone number
      "status": "pending", //Status of the transaction (pending || success || error)
      "statusMessage": "pending", //Message explaining what is exactly hapennig for the transaction
      "statusMessageCode": 202, //Specific code of the exact state of the transaction
      "startTimestampInSecond": 1655968725, //Timestamp in second of the moment when you initiated the transaction
      "endTimestampInSecond": 0, //Timestamp in second of the moment when a tranaction is finished
      "amount": 2000, //Amount in local currency that the recipient will receive
      "originalAmount": 200 //Amount you specified the user should receive in your request
    }
  }
  ```
</details>

<details>
  <summary>Status of collection </summary>
  
  #### Collection transaction status model

  ```json
  {
    "success": true, //status of the request processing
    "message": "", //message of the request processing
    "transaction": {
      "reference": "b8f9aeb4-ca07-4438-b9ce-aa975a39d15d", //reference of the transaction in our system
      "senderPhoneNumber": "22951945229", //Phone number of the money sender
      "senderCountryCode": "BJ", //Country code of the money sender
      "senderOperator": "MTN", //Operator of the money sender
      "senderFirstName": "Jean", //Firstname of the money sender
      "senderLastName": "EVERICH", //Lastname of the money sender
      "originalCurrency": "XOF", //Currency of payment you specified in your request
      "currency": "XOF", //Local currency of the sender phone number
      "status": "success", //Message explaining what is exactly hapennig for the transaction
      "statusMessage": "Successfully processed transaction",
      "statusMessageCode": 200, //Specific code of the exact state of the transaction
      "startTimestampInSecond": 1655968250, //Timestamp in second of the moment when you initiated the transaction
      "endTimestampInSecond": 1655968350, //Timestamp in second of the moment when a tranaction is finished
      "amount": 10, //Amount in local currency that the sender will pay
      "originalAmount": 10 //Amount you specified the user should pay in your request
      "transactionType": "payment" //type of the transaction (payment || deposit)
    }
  }
  ```
</details>

<details>
  <summary>Status of disbursement </summary>
  
  #### Disbursement transaction status model

  ```json
  {
    "success": true, //status of the request processing
    "message": "", //message of the request processing
    "transaction": {
      "reference": 9d621c8f-5a8e-4f88-ad24-984fb47a8ce8//reference of the transaction in our system
      "recipientPhoneNumber": "22951945229", //Phone number of the money recipient
      "recipientOperator": "MTN", //Operator of the money recipient
      "recipientCountryCode": "BJ", //Country code of the money recipient
      "originalCurrency": "XOF", //Currency of deposit you specified in your request
      "currency": "XOF", //Local currency of the sender phone number
      "status": "success", //Message explaining what is exactly hapennig for the transaction
      "statusMessage": "Successfully processed transaction",
      "statusMessageCode": 200, //Specific code of the exact state of the transaction
      "startTimestampInSecond": 1655968725, //Timestamp in second of the moment when you initiated the transaction
      "endTimestampInSecond": 1655968825, //Timestamp in second of the moment when a tranaction is finished
      "amount": 2000, //Amount in local currency that the recipient will receive
      "originalAmount": 200 //Amount you specified the user should receive in your request
      "transactionType": "deposit" //type of the transaction (payment || deposit)
    }
  }
  ```
</details>

<details>
  <summary>balance </summary>
  
  #### Balance Response model

  ```json
  {
    "success": true, //status of the request processing
    "message": "", //message of the request processing
    "balance": [ //array of all balances by currency
      {
        "amount": 0, //amount
        "currency": "XOF" //balance 
      }
    ]
  }
  ```
</details>


### Error codes

This are error values that can be contained in property ``errorCode`` of a request which go an error

| Code    | message                                                       |
| ------- | ------------------------------------------------------------- |
| 400-000 | Endpoint not available                                        |
| 400-100 | Parameter senderFirstName not found in the request            |
| 400-101 | Parameter senderFirstName contain invalid data                |
| 400-102 | Parameter senderLastName not found in the request             |
| 400-103 | Parameter senderLastName contain invalid data                 |
| 400-104 | Parameter senderPhoneNumber not found in the request          |
| 400-105 | Parameter senderPhoneNumber contain invalid data              |
| 400-106 | Parameter amount not found in the request                     |
| 400-107 | Parameter amount contain invalid data                         |
| 400-110 | Parameter recipientPhoneNumber not found in the request       |
| 400-111 | Parameter recipientPhoneNumber contain invalid data           |
| 400-112 | Parameter senderPhoneNumber sent have bad operator            |
| 400-113 | Parameter senderPhoneNumber sent have bad operator            |
| 400-114 | Parameter senderPhoneNumber contain invalid data              |
| 400-115 | Parameter senderPhoneNumber contain invalid data              |
| 400-116 | Currency you enter in the request is not supported            |
| 400-117 | Parameter currency not found in the request                   |
| 400-118 | Parameter currency contain invalid data                       |
| 401-100 | Incorrect api key in the request                              |
| 404-100 | No transaction found with the reference in the request        |
| 403-100 | Not enough money in your balance for a deposit                |
| 500-000 | Sorry an error occured on the server. try again.              |
| 500-001 | Sorry an error occured when converting currencies. try again. |


<br/>
<br/>

# Webhook

Webhooks are an important part of your payment integration. They allow Chipdeals notify you about events that happen on your account, such as a successful payment or a failed transaction.

A ***webhook URL*** is an endpoint on your server where you can receive notifications about such events. When an event occurs, we'll make a ***POST*** request to that endpoint, with a ***JSON body*** containing the details about the event, including the type of event and the data associated with it.

<br/>

## Structure of a webhook payload
All webhook payloads follow the same basic structure:

* an `event` field describing the type of event
* a `data` object. The contents of this object will vary depending on the event, but typically it will contain details of the event, including:
    * a `reference` containing the ID of the transaction
    * a `status` describing the status of the transaction. possible values are `success`, `pending` or `error`
    * a `statusMessageCode`, containing a specific code that identify an exact state of the transaction. [See all `statusMessageCode`](#Status-Message-Code) 
    * a `statusMessage`, cantaining an human undertandable descrption of the exact state of the transaction
    * transaction details
    
[Here are some sample webhook payloads for transfers and payments](#Webhook-sample)

<br/>

## Implementing a webhook

Creating a webhook endpoint on your server is the same as writing any other API endpoint, but there are a few important details to note:

**Verifying webhook signatures**
When enabling webhooks, you have the option to set a ***secret hash***. Since webhook URLs are publicly accessible, the secret hash allows you to verify that incoming requests are from Chipdeals. You can specify any value as your secret hash, but we recommend something random. We also recommend to store it as an environment variable on your server.

If you specify a secret hash, we'll include it in our request to your webhook URL, in a header called `verif-hash`. In the webhook endpoint, check if the `verif-hash` header is present and that it matches the secret hash you set. If the header is missing, or the value doesn't match, you can discard the request, as it isn't from Chipdeals.

**Responding to webhook requests**
To acknowledge receipt of a webhook, your endpoint **must** return a `200` HTTP status code. Any other response codes, including `3xx` codes, will be treated as a failure. **We don't care about the response body or headers.**

Be sure to enable webhook retries on your dashboard. If we don't get a 200 status code (for example, if your server is unreachable), we'll retry the webhook call every 90 minutes for the next 36 hours.

<br/>

## Webhook payload sample

### Collectiton state changed webhook payload sample
```json
{
  "event": "collection.stateChanged", 
  "data": {
    "reference": "dd1e2d17-5c21-4964-b58d-198fd2aac150",
    "status": "success", 
    "statusMessageCode": 200,
    "statusMessage": "transaction successfully processed", 
    "senderPhoneNumber": "22951010591", //Phonenumeber you speciy in your collection request
    "senderFirstName": "Jean", //User firstname you specified in your collect request
    "senderLastName": "EVERICH", //User lastname you specified in your collect request
    "currency": "XOF", //Currency of the transaction. 
    "amount": 2000, //Amount of the transaction
    "senderOperator": "MTN", // Mobile Money wallet operator
    "startTimestampInSecond": 1638087167, // Timestamp in second of whe you init the transaction
    "endTimestampInSecond": 1638087167 // Timestamp in second of when the transaction is finished
  }
}
```

### Disbursement state changed webhook payload sample
```json
{
  "event": "deposit.stateChanged", 
  "data": {
    "reference": "dd1e2d17-5c21-4964-b58d-198fd2aac150", 
    "status": "success", 
    "statusMessageCode": 200, 
    "statusMessage": "success", 
    "recipientPhoneNumber": "22951010591", //Phone number you specify in your deposit request
    "currency": "XOF", //Currency of the transaction. 
    "amount": 2000, //Amount of the transaction
    "senderOperator": "MTN", // Mobile Money wallet operator
    "startTimestampInSecond": 1638087167, // Timestamp in second of whe you init the transaction
    "endTimestampInSecond": 1638087167 //Timestamp in second of when the transaction is finished
  }
}
```

<br/>

# More Info

## Collection and disburssement parametters

### - ``senderPhoneNumber`` or ``recipientPhoneNumber`` parameter

You specify the ``phoneNumber`` of a collection with parametter `senderPhoneNumber: sting` in your http request

And for a disbursement you specify ``phoneNumber`` with parametter `recipientPhoneNumber: sting` in your request

Those parametters are locking for a string containing the phone number of the money sender for collection and the money recipient for disbursement.

The PhoneNumber should respect this format:

``XXXOOOOOOOO``

where the three ``X`` represent the country phone préfix (``229`` | ``237`` for example), and the ``O`` are the phone number in the country. The number of `O` can change with the country but the prefix should be 3.

### - ``currency`` parameter 
You specify the currency of a transaction with the parammeter `currency: string`

The currency should be a string of 3 character.

If you add a currency that is not the currency localy used in the country of the phone number of the transaction, we will convert the amount of the transaction into local currency.

Our conversion rate evoluate like the market.

### - ``amount`` parameter 
You specify the amount of a transaction with the parametter `amount: number`

This parametter should be a number. 

We will check if the currency that you specify for the transaction is the local currency of the phone number's country and then we will convert the amount into local currency before performing the transaction with the customer

### - ``senderFistName`` and ``senderLastName`` parameters 

You respectively specify ``firstName`` and ``lastName`` with properties ``senderFirstName: string`` and ``senderLastName``

Those parametters are required to perform secured collection request.

Disbursement doesn't need them.

If  ``senderFirstName`` and ``senderLastName`` are not specified in collection request, you will get an error as response

<br/>

## Status Message Code
#### You have here the values of the property ``statusMessageCode`` contained in [``transactionData`` Object](#transactionData)

| Code | Relative Status | Meaning                                                            |
|:----:| --------------- | ------------------------------------------------------------------ |
| 200  | ✔️ -- success   | Transaction successful                                             |
| 201  | 🕟 -- pending   | Data in validation                                                 |
| 202  | 🕟 -- pending   | Transaction pending                                                |
| 203  | 🕟 -- pending   | Data are validated, server is working                              |
| 204  | 🕟 -- pending   | Waiting for ussd push validation                                   |
| 400  | ❌ -- error     | Incorrect data enter in the request                                |
| 401  | ❌ -- error     | Parameters not complete                                            |
| 402  | ❌ -- error     | Payment PhoneNumber is not correct                                 |
| 403  | ❌ -- error     | Deposit PhoneNumber is not correct                                 |
| 404  | ❌ -- error     | Timeout in USSD PUSH/ Cancel in USSD PUSH                          |
| 406  | ❌ -- error     | Payment phoneNumber got is not for mobile money wallet             |
| 460  | ❌ -- error     | Payer’s payment account balance is low                             |
| 461  | ❌ -- error     | An error occured while paying                                      |
| 462  | ❌ -- error     | This kind of transaction is not supported yet, processor not found |
| 5XX  | ⛔️ -- error    | An unknow error occured on the api                                 |

<br/>

## Sandbox tests

You can use your `test apikey` or all users test apikey : `test_FOdigzgSopV8GZggZa89` to make sandbox requsts.

All valid phone number you used in sandbox will send you valid responses as it was the live mode.

For exemple, if you use the phone number `22951010581` you will get a response with status message code `201` (what means `pending` see more [here](#Status-Message-Code)). Some seconds laters, you will get status messages codes `202`, `203` and `200` (`200` means success).

It is the same thing for others phone number with a small exception to allow errors case handling by implementor.



| Sandbox Phone number | -> final status message code |
| -------------------- | ----------------------------:|
| 22951010402          |                          402 |
| 22951010403          |                          403 |
| 22951010404          |                          404 |
| 22951010460          |                          460 |
| 22951010461          |                          461 |
| 22951010462          |                          462 |
| Any other valid      |                          200 |

For **Live `apiKey`** and **Live Responses** requests **[Contact us](#Contact-us)**

<br/>


## Limitation
### Unsecured collect limitation

When you are making a collection request, you have posibility to specify or not the payer's fistname and lastname.
By not specifying `firstName` and `lastName`, you can make quick test.

But when `firstName` and `lastName` are not specified your collection request is not secured. 
And you are alowed to perform at most 3 unsecured collection resquest per day. More request will be just blocked.

You cannot perform unsecured collection request for more than 500 XOF.

<!-- 
### Test apiKey limitation

With test apiKey, you can make so many request you wish, but none will be connected to real user's wallet.
 -->
<!-- To connect to user's wallet with test apiKey you must specify it when you initialize apikey.

#### Sample Initialize user wallet connection with test apiKey
 -->
<!-- <iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C184%2C195%2C0%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=10px&dsblur=12px&wc=true&wa=true&pv=31px&ph=31px&ln=true&fl=1&fm=Fira+Code&fs=14px&lh=133%25&si=false&es=2x&wm=false&code=const%2520momo%2520%253D%2520require%28%27@chipdeals/momo-api%27%29%253B%250Amomo.setApiKey%28%250A%2520%2520%27test_FOdigzgSopV8GZggZa89%27%252C%250A%2520%2520%257B%2520enableTestRealWalletConnection%253A%2520true%2520%257D%250A%29%253B%250A"
  style="width: 862px; height: 250px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe> -->
<!-- 

```javascript=
const momo = require('@chipdeals/momo-api');
momo.setApiKey(
  'test_FOdigzgSopV8GZggZa89',
  { enableTestRealWalletConnection: true }
);
```

When you connect to real user wallet with test apiKey, you cannot perform any collection nor disbusement for more than 1 XOF. More than 1 XOF will just be blocked -->

<br/>

# Contact us

### Call us or write us to [get your apikey](#Contact-us) and start getting payment

<br/>

E-mail: products@chipdeals.me
Website: https://chipdeals.me
Phone: +22990630401
Whatsapp: +22990630401

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

#
Copyright (C) 2022 Chipdeals Inc - https://chipdeals.me
