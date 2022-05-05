# Mobile Money API for, [Android](#), [Javascript](#), [Nodejs](#) and [PHP](#)

![Nodejs](https://img.shields.io/badge/Node.js-43853D?style=for-the-badge&logo=node.js&logoColor=white)

**Chipdeals-momo-api** is a Mobile Money API that allows you to build a quick, simple and excellent payment experience in your web and native app.This the official **Nodejs Library**

Accept Payments in your node app.

**You can [request payment](#Collect-Money) and [send money](#Disburse-Money) to any mobile money account**

# Requirements

*Node 8, 10 or higher*

# Installation
```bash
npm install @chipdeals/momo-api --save
# or
yarn add @chipdeals/momo-api
```

# Quick Start

**Initialize Chipdeals Momo API with your API Key ([*Get apikey here*](#)) and start**

```javascript=
const momo = require('@chipdeals/momo-api');
momo.setApiKey('test_FOdigzgSopV8GZggZa89');

//Collect 500 XOF from the +229510150181 Mobile Money wallet.
momo
  .collect()
  .amount(500)
  .currency('XOF')
  .from('229510150181')
  .create();

//Send 2000 XOF to the +229510150181 Mobile Money wallet.
momo
  .deposit()
  .amount(2000)
  .currency('XOF')
  .to('229510150181')
  .create();
```



# Usage
The package needs to be configured with your **account's API key**, which is available in the when you get access to Chipdeals Sandbox.

[***You can get you apiKey here***](#)

## Collect Money
[Collect limitation](#collect-limitation)

### Simple collection
For example to request 2000 XOF from the ***+229510150181*** Mobile Money wallet, the following code can be used

```javascript=
const momo = require('@chipdeals/momo-api');
momo.setApiKey('test_FOdigzgSopV8GZggZa89');

momo
  .collect()
  .amount(2000)
  .currency('XOF')
  .from('229510150181')
  .fistName('Jean')
  .lastName('Luc')
  .create(transacrionReference => console.log(transacrionReference));
```

### Collect with a [webhook](#Webhook) to get response as soon as the payment is processed. 

```javascript=4
momo
  .collect()
  .amount(2000)
  .currency('XOF')
  .from('229510150181')
  .fistName('Jean')
  .lastName('Luc')
  .webhook('https://mydomain/payment-status')
  .create(transacrionReference => console.log(transacrionReference));
```
[*See webhook you get*](#Collectiton-state-changed-webhook-payload-sample)

### Make Collection using Callbacks. 

```javascript=4
momo
  .collect()
  .amount(2000)
  .currency('XOF')
  .from('229510150181')
  .fistName('Jean')
  .lastName('Luc')
  .create(transacrionReference => console.log(transacrionReference))
  .onStatusChanged(paymentData => console.log(paymentData))
  .onSuccess(paymentData => console.log(paymentData))
  .onError(paymentData => console.error(paymentData));
```

[*See what you receive as callback parametters*](#Collection-callback-data-sample)

## Disburse Money

### Simple Disbursement

You can also send 2000 XOF to the ***+229510150181*** Mobile Money wallet, with the following code

```javascript=
const momo = require('@chipdeals/momo-api');
momo.setApiKey('test_FOdigzgSopV8GZggZa89');

momo
  .deposit()
  .amount(2000)
  .currency('XOF')
  .to('229510150181')
  .create(transacrionReference => console.log(transacrionReference));
```

### Disburse with a [webhook](#Webhook) to get response as soon as the deposit is processed. 

```javascript=4
momo
  .deposit()
  .amount(2000)
  .currency('XOF')
  .to('229510150181')
  .webhook('https://mydomain/deposit-status')
  .create(transacrionReference => console.log(transacrionReference));
```
[*See webhook you get*](#Disbursement-state-changed-webhook-payload-sample)

### Make Disbursement using Callbacks. 

```javascript=4
momo
  .deposit()
  .amount(2000)
  .currency('XOF')
  .to('229510150181')
  .create(transacrionReference => console.log(transacrionReference))
  .onSuccess(depositData => console.log(depositData))
  .onError(depositData => console.error(depositData));
```
[*See what you receive as callback parametters*](#Disbursement-callback-data-sample)


## Get transaction status

Get status of a transaction of reference `dd1e2d17-5c21-4964-b58d-198fd2aac150`

```javascript=
const momo = require('@chipdeals/momo-api');
momo.setApiKey('test_FOdigzgSopV8GZggZa89');

const reference = "dd1e2d17-5c21-4964-b58d-198fd2aac150";
momo
  .status(reference)
  .then((transactionData)=>console.log(transactionData))
```

<details>
  <summary>Collection transactionData Sample </summary>
  
  ```json
  {
    "type": "collection", //the type of the transaction
    "reference": "dd1e2d17-5c21-4964-b58d-198fd2aac150",
    "status": "success", 
    "statusMessageCode": 200,
    "statusMessage": "transaction successfully processed", 
    "senderPhoneNumber": "22951010580", //Phonenumeber you speciy in your collection request
    "senderFirstName": "Euler", //User firstname you specified in your collect request
    "senderLastName": "Dougbe", //User lastname you specified in your collect request
    "currency": "XOF", //Currency of the transaction. 
    "amount": 2000, //Amount of the transaction
    "senderOperator": "MTN", // Mobile Money wallet operator
    "startTimestampInSecond": 1638087167, // Timestamp in second of whe you init the transaction
    "endTimestampInSecond": 1638087167 // Timestamp in second of when the transaction is finished
  }
  ```
</details>

<details>
  <summary>Disbursement transactionData Sample </summary>
  
  ```json
  {
    "type": "deposit", //the type of the transaction
    "reference": "dd1e2d17-5c21-4964-b58d-198fd2aac150", 
    "status": "success", 
    "statusMessageCode": 200, 
    "statusMessage": "success", 
    "recipientPhoneNumber": "22951010580", //Phone number you specify in your deposit request
    "currency": "XOF", //Currency of the transaction. 
    "amount": 2000, //Amount of the transaction
    "recipientOperator": "MTN", // Mobile Money wallet operator
    "startTimestampInSecond": 1638087167, // Timestamp in second of whe you init the transaction
    "endTimestampInSecond": 1638087167 //Timestamp in second of when the transaction is finished
  }
  ```
</details>


## Get Your balance

Get your Chipdeals account's balance

```javascript=
const momo = require('@chipdeals/momo-api');
momo.setApiKey('test_FOdigzgSopV8GZggZa89');

momo
  .balance()
  .then((balance)=>console.log(balance))
```

Balance sample:

```json
[
  {
    "amount": 2000,
    "currency": "XOF"
  },
  {
    "amount": 10.7,
    "currency": "USD"
  },
]
```

## Manage logs

By default, logs are enabled for `test apiKey` and disabled for `live apiKey`.
You can change the default setting like this

```javascript=
const momo = require('@chipdeals/momo-api');


momo.setApiKey('test_FOdigzgSopV8GZggZa89'); //logs enable by default
momo.log(false)//to enable logs for test


momo.setApiKey('live_bjMBMJVmhlmNMjbmNJM6'); //logs disabled by default
momo.log(true)//to disable logs for live
//or
momo.log()//to enable logs for live

```


# Webhook

Webhooks are an important part of your payment integration. They allow Chipdeals notify you about events that happen on your account, such as a successful payment or a failed transaction.

A ***webhook URL*** is an endpoint on your server where you can receive notifications about such events. When an event occurs, we'll make a ***POST*** request to that endpoint, with a ***JSON body*** containing the details about the event, including the type of event and the data associated with it.

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

## Implementing a webhook

Creating a webhook endpoint on your server is the same as writing any other API endpoint, but there are a few important details to note:

**Verifying webhook signatures**
When enabling webhooks, you have the option to set a ***secret hash***. Since webhook URLs are publicly accessible, the secret hash allows you to verify that incoming requests are from Chipdeals. You can specify any value as your secret hash, but we recommend something random. We also recommend to store it as an environment variable on your server.

If you specify a secret hash, we'll include it in our request to your webhook URL, in a header called `verif-hash`. In the webhook endpoint, check if the `verif-hash` header is present and that it matches the secret hash you set. If the header is missing, or the value doesn't match, you can discard the request, as it isn't from Chipdeals.

**Responding to webhook requests**
To acknowledge receipt of a webhook, your endpoint **must** return a `200` HTTP status code. Any other response codes, including `3xx` codes, will be treated as a failure. **We don't care about the response body or headers.**

Be sure to enable webhook retries on your dashboard. If we don't get a 200 status code (for example, if your server is unreachable), we'll retry the webhook call every 90 minutes for the next 36 hours.

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
    "senderPhoneNumber": "22951010580", //Phonenumeber you speciy in your collection request
    "senderFirstName": "Euler", //User firstname you specified in your collect request
    "senderLastName": "Dougbe", //User lastname you specified in your collect request
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
    "recipientPhoneNumber": "22951010580", //Phone number you specify in your deposit request
    "currency": "XOF", //Currency of the transaction. 
    "amount": 2000, //Amount of the transaction
    "recipientOperator": "MTN", // Mobile Money wallet operator
    "startTimestampInSecond": 1638087167, // Timestamp in second of whe you init the transaction
    "endTimestampInSecond": 1638087167 //Timestamp in second of when the transaction is finished
  }
}
```

# Callback

Call back allow to be notified in your code as soon as events happen about your transaction, such as a successful payment or a failed transaction.

When an event occurs, we'll call concerned callback function and will pass a single parametter containing the details about the event, the state of the transaction and the data associated with it.

## Structure of callback data
All webhook payloads (except virtual card debits) follow the same basic structure:

* an `event` field describing the type of event
* a `data` object. The contents of this object will vary depending on the event, but typically it will contain details of the event, including:
    * a `reference` containing the ID of the transaction
    * a `statusMessageCode`, containing a specific code that identify an exact state of the transaction. [See all `statusMessageCode`](#Status-Message-Code) 
    * a `statusMessage`, cantaining an human undertandable descrption of the exact state of the transaction
    * transaction details


## Callback data sample

### Collection callback data sample
```json
{
  "reference": "dd1e2d17-5c21-4964-b58d-198fd2aac150", 
  "status": "success",
  "statusMessageCode": 200,
  "statusMessage": "transaction successfully processed", 
  "senderPhoneNumber": "22951010580", //Phonenumeber you speciy in your collection request
  "senderFirstName": "Euler", //User firstname you specified in your collect request
  "senderLastName": "Dougbe", //User lastname you specified in your collect request
  "currency": "XOF", //Currency of the transaction. 
  "amount": 2000, //Amount of the transaction
  "senderOperator": "MTN", // Mobile Money wallet operator
  "startTimestampInSecond": 1638087167, // Timestamp in second of whe you init the transaction
  "endTimestampInSecond": 1638087167 // Timestamp in second of when the transaction is finished
}
```

### Disbursement callback data sample
```json
{
  "reference": "dd1e2d17-5c21-4964-b58d-198fd2aac150", // reference of the transaction Chipdeas Momo Api
  "status": "success",
  "statusMessageCode": 200, 
  "statusMessage": "transaction successfully processed", 
  "recipientPhoneNumber": "22951010580", //Phone number you specify in your deposit request
  "currency": "XOF", //Currency of the transaction. 
  "amount": 2000, //Amount of the transaction
  "recipientOperator": "MTN", // Mobile Money wallet operator
  "startTimestampInSecond": 1638087167, // Timestamp in second of whe you init the transaction
  "endTimestampInSecond": 1638087167 //Timestamp in second of when the transaction is finished
}
```

# More Info

## Status Message Code

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


# Limitation
## Unsecured collect limitation

When you are making a collection request, you have posibility to specify or not the payer's fistname and lastname.
By not specifying `firstName` and `lastName`, you can make quick test.

But when `firstName` and `lastName` are not specified your collection request is not secured. 
And you are alowed to perform at most 3 unsecured collection resquest per day. More request will be just blocked.

You cannot perform unsecured collection request for more than 500 XOF.

## Test apiKey limitation

With test apiKey, you can make so many request you wish, but none will be connected to real user's wallet.

To connect to user's wallet with test apiKey you must specify it when you initialize apikey.

#### Sample Initialize user wallet connection with test apiKey
```javascript=
const momo = require('@chipdeals/momo-api');
momo.setApiKey(
  'test_FOdigzgSopV8GZggZa89',
  { enableTestRealWalletConnection: true }
);
```

When you connect to real user wallet with test apiKey, you cannot perform any collection nor disbusement for more than 1 XOF. More than 1 XOF will just be blocked


---
Copyright (C) 2022 Chipdeals Inc
