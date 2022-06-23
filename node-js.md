# Mobile Money [REST API](https://github.com/Chipdeals/Momo-Api), and [Nodejs Library](https://github.com/Chipdeals/Momo-Api/blob/main/node-js.md) documentation

![Nodejs](https://img.shields.io/badge/Node.js-43853D?style=for-the-badge&logo=node.js&logoColor=white)

**Chipdeals-momo-api** is a Mobile Money API that allows you to build a quick, simple and excellent payment experience in your web and native app.This the official **Nodejs Library**


<div style="text-align: center">
<img src="https://www.chipdeals.me/images/code.gif" alt="Fast integration" style="width:450px; border-radius: 5px 5px 0 0"/>
    <br/>
Accept Payments in your node app.
</div>


**You can [request payment](#Collect-Money) and [send money](#Disburse-Money) to any mobile money account**

# Requirements

*Node 8, 10 or higher*

<br/>

# Installation
```bash
npm install @chipdeals/momo-api --save
# or
yarn add @chipdeals/momo-api
```

<br/>

# Quick Start

**Initialize Chipdeals Momo API with your API Key ([*Get apikey here*](#Contact-us)) and start**
<!-- 
<iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C184%2C195%2C0%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=10px&dsblur=12px&wc=true&wa=true&pv=31px&ph=31px&ln=true&fl=1&fm=Fira+Code&fs=14px&lh=133%25&si=false&es=2x&wm=false&code=const%2520momo%2520%253D%2520require%28%27@chipdeals/momo-api%27%29%253B%250Amomo.setApiKey%28%27test_FOdigzgSopV8GZggZa89%27%29%253B%250A%250A%252F%252FCollect%2520500%2520XOF%2520from%2520the%2520%252B22951010591%2520Mobile%2520Money%2520wallet.%250Amomo%250A%2520%2520.collect%28%29%250A%2520%2520.amount%28500%29%250A%2520%2520.currency%28%27XOF%27%29%250A%2520%2520.from%28%2722951010591%27%29%250A%2520%2520.create%28%29%253B%250A%250A%252F%252FSend%25202000%2520XOF%2520to%2520the%2520%252B22951010591%2520Mobile%2520Money%2520wallet.%250Amomo%250A%2520%2520.deposit%28%29%250A%2520%2520.amount%282000%29%250A%2520%2520.currency%28%27XOF%27%29%250A%2520%2520.to%28%2722951010591%27%29%250A%2520%2520.create%28%29%253B%250A"
  style="width: 862px; height: 571px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe> -->


```javascript=
const momo = require('@chipdeals/momo-api');
momo.setApiKey('test_FOdigzgSopV8GZggZa89');

//Collect 500 XOF from the +22951010591 Mobile Money wallet.
momo
  .collect()
  .amount(500) //Amount of the transaction
  .currency('XOF') // Any valid currency
  .from('22951010591') // Sender phone number with country code préfix
  .create();

//Send 2000 XOF to the +22951010591 Mobile Money wallet.
momo
  .deposit()
  .amount(2000) //Amount of the transaction
  .currency('XOF') // Any valid currency
  .to('22951010591') // Recipient phone number with country code préfix
  .create();
```

###### Quick check possible responses with [sandbox tests](#Sandbox-tests)

<br/>

# Usage
The package needs to be configured with your **account's API key**, which is available in the when you get access to Chipdeals Sandbox.

[***You can get your apiKey here***](#Contact-us)

<br/>

## Collect Money
[Collect limitation](#collect-limitation)

### Simple collection
For example to request 2000 XOF from the ***+22951010591*** Mobile Money wallet, the following code can be used

<!-- <iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C184%2C195%2C0%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=10px&dsblur=12px&wc=true&wa=true&pv=31px&ph=31px&ln=true&fl=1&fm=Fira+Code&fs=14px&lh=133%25&si=false&es=2x&wm=false&code=const%2520momo%2520%253D%2520require%28%27@chipdeals/momo-api%27%29%253B%250Amomo.setApiKey%28%27test_FOdigzgSopV8GZggZa89%27%29%253B%250A%250Amomo%250A%2520%2520.collect%28%29%250A%2520%2520.amount%282000%29%250A%2520%2520.currency%28%27XOF%27%29%250A%2520%2520.from%28%2722951010591%27%29%250A%2520%2520.fistName%28%27Jean%27%29%250A%2520%2520.lastName%28%27Luc%27%29%250A%2520%2520.create%28transacrionReference%2520%253D%253E%2520console.log%28transacrionReference%29%29%253B%250A"
  style="width: 862px; height: 360px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe> -->

```javascript=
const momo = require('@chipdeals/momo-api');
momo.setApiKey('test_FOdigzgSopV8GZggZa89');

momo
  .collect()
  .amount(2000) //Amount of the transaction
  .currency('XOF') // Any valid currency
  .from('22951010591') // Sender phone number with country code préfix
  .fistName('Jean') // First name of the sender
  .lastName('Luc') // Last name of the sender
  .create(transacrionReference => console.log(transacrionReference));
```

### Collect with a [webhook](#Webhook) to get response as soon as the payment is processed. 

<!-- <iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C184%2C195%2C0%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=10px&dsblur=12px&wc=true&wa=true&pv=31px&ph=31px&ln=true&fl=4&fm=Fira+Code&fs=14px&lh=133%25&si=false&es=2x&wm=false&code=momo%250A%2520%2520.collect%28%29%250A%2520%2520.amount%282000%29%250A%2520%2520.currency%28%27XOF%27%29%250A%2520%2520.from%28%2722951010591%27%29%250A%2520%2520.fistName%28%27Jean%27%29%250A%2520%2520.lastName%28%27Luc%27%29%250A%2520%2520.webhook%28%27https%253A%252F%252Fmydomain%252Fpayment-status%27%29%250A%2520%2520.create%28transacrionReference%2520%253D%253E%2520console.log%28transacrionReference%29%29%253B%250A"
  style="width: 862px; height: 320px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe> -->

```javascript=4
momo
  .collect()
  .amount(2000) //Amount of the transaction
  .currency('XOF') // Any valid currency
  .from('22951010591') // Sender phone number with country code préfix
  .fistName('Jean') // First name of the sender
  .lastName('Luc') // Last name of the sender
  .webhook('https://mydomain/payment-status')
  .create(transacrionReference => console.log(transacrionReference));
```

[*See webhook you get*](#Collectiton-state-changed-webhook-payload-sample)

### Make Collection using Callbacks. 

<!-- <iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C184%2C195%2C0%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=10px&dsblur=12px&wc=true&wa=true&pv=31px&ph=31px&ln=true&fl=4&fm=Fira+Code&fs=14px&lh=133%25&si=false&es=2x&wm=false&code=momo%250A%2520%2520.collect%28%29%250A%2520%2520.amount%282000%29%250A%2520%2520.currency%28%27XOF%27%29%250A%2520%2520.from%28%2722951010591%27%29%250A%2520%2520.fistName%28%27Jean%27%29%250A%2520%2520.lastName%28%27Luc%27%29%250A%2520%2520.create%28transacrionReference%2520%253D%253E%2520console.log%28transacrionReference%29%29%250A%2520%2520.onStatusChanged%28paymentData%2520%253D%253E%2520console.log%28paymentData%29%29%250A%2520%2520.onSuccess%28paymentData%2520%253D%253E%2520console.log%28paymentData%29%29%250A%2520%2520.onError%28paymentData%2520%253D%253E%2520console.error%28paymentData%29%29%253B%250A"
  style="width: 862px; height: 360px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe> -->


```javascript=4
momo
  .collect()
  .amount(2000) //Amount of the transaction
  .currency('XOF') // Any valid currency
  .from('22951010591') // Sender phone number with country code préfix
  .fistName('Jean') // First name of the sender
  .lastName('Luc') // Last name of the sender
  .create(transacrionReference => console.log(transacrionReference))
  .onStatusChanged(paymentData => console.log(paymentData))
  .onSuccess(paymentData => console.log(paymentData))
  .onError(paymentData => console.error(paymentData));
```

[*See what you receive as callback parametters*](#Collection-callback-data-sample)

<br/>

## Disburse Money

### Simple Disbursement

You can also send 2000 XOF to the ***+22951010591*** Mobile Money wallet, with the following code

<!-- <iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C184%2C195%2C0%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=10px&dsblur=12px&wc=true&wa=true&pv=31px&ph=31px&ln=true&fl=1&fm=Fira+Code&fs=14px&lh=133%25&si=false&es=2x&wm=false&code=const%2520momo%2520%253D%2520require%28%27@chipdeals/momo-api%27%29%253B%250Amomo.setApiKey%28%27test_FOdigzgSopV8GZggZa89%27%29%253B%250A%250Amomo%250A%2520%2520.deposit%28%29%250A%2520%2520.amount%282000%29%250A%2520%2520.currency%28%27XOF%27%29%250A%2520%2520.to%28%2722951010591%27%29%250A%2520%2520.create%28transacrionReference%2520%253D%253E%2520console.log%28transacrionReference%29%29%253B%250A"
  style="width: 862px; height: 320px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe> -->


```javascript=
const momo = require('@chipdeals/momo-api');
momo.setApiKey('test_FOdigzgSopV8GZggZa89');

momo
  .deposit()
  .amount(2000) //Amount of the transaction
  .currency('XOF') // Any valid currency
  .to('22951010591') // Recipient phone number with country code préfix
  .create(transacrionReference => console.log(transacrionReference));
```

### Disburse with a [webhook](#Webhook) to get response as soon as the deposit is processed. 
<!-- 
<iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C184%2C195%2C0%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=10px&dsblur=12px&wc=true&wa=true&pv=31px&ph=31px&ln=true&fl=4&fm=Fira+Code&fs=14px&lh=133%25&si=false&es=2x&wm=false&code=momo%250A%2520%2520.deposit%28%29%250A%2520%2520.amount%282000%29%250A%2520%2520.currency%28%27XOF%27%29%250A%2520%2520.to%28%2722951010591%27%29%250A%2520%2520.webhook%28%27https%253A%252F%252Fmydomain%252Fdeposit-status%27%29%250A%2520%2520.create%28transacrionReference%2520%253D%253E%2520console.log%28transacrionReference%29%29%253B%250A"
  style="width: 862px; height: 280px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe> -->


```javascript=4
momo
  .deposit()
  .amount(2000) //Amount of the transaction
  .currency('XOF') // Any valid currency
  .to('22951010591') // Recipient phone number with country code préfix
  .webhook('https://mydomain/deposit-status')
  .create(transacrionReference => console.log(transacrionReference));
```
[*See webhook you get*](#Disbursement-state-changed-webhook-payload-sample)

### Make Disbursement using Callbacks.

<!-- <iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C184%2C195%2C0%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=10px&dsblur=12px&wc=true&wa=true&pv=31px&ph=31px&ln=true&fl=4&fm=Fira+Code&fs=14px&lh=133%25&si=false&es=2x&wm=false&code=momo%250A%2520%2520.deposit%28%29%250A%2520%2520.amount%282000%29%250A%2520%2520.currency%28%27XOF%27%29%250A%2520%2520.to%28%2722951010591%27%29%250A%2520%2520.create%28transacrionReference%2520%253D%253E%2520console.log%28transacrionReference%29%29%250A%2520%2520.onSuccess%28depositData%2520%253D%253E%2520console.log%28depositData%29%29%250A%2520%2520.onError%28depositData%2520%253D%253E%2520console.error%28depositData%29%29%253B%250A"
  style="width: 862px; height: 300px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe> -->


```javascript=4
momo
  .deposit()
  .amount(2000) //Amount of the transaction
  .currency('XOF') // Any valid currency
  .to('22951010591') // Recipient phone number with country code préfix
  .create(transacrionReference => console.log(transacrionReference))
  .onSuccess(depositData => console.log(depositData))
  .onError(depositData => console.error(depositData));
```
[*See what you receive as callback parametters*](#Disbursement-callback-data-sample)


<br/>

## Get transaction status

Get status of a transaction of reference `dd1e2d17-5c21-4964-b58d-198fd2aac150`

<!-- <iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C184%2C195%2C0%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=10px&dsblur=12px&wc=true&wa=true&pv=31px&ph=31px&ln=true&fl=1&fm=Fira+Code&fs=14px&lh=133%25&si=false&es=2x&wm=false&code=const%2520momo%2520%253D%2520require%28%27@chipdeals/momo-api%27%29%253B%250Amomo.setApiKey%28%27test_FOdigzgSopV8GZggZa89%27%29%253B%250A%250Aconst%2520reference%2520%253D%2520%2522dd1e2d17-5c21-4964-b58d-198fd2aac150%2522%253B%250Amomo%250A%2520%2520.status%28reference%29%250A%2520%2520.then%28%28transactionData%29%253D%253Econsole.log%28transactionData%29%29%250A"
  style="width: 862px; height: 280px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe> -->

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
    "senderPhoneNumber": "22951010591", //Phonenumeber you speciy in your collection request
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
    "recipientPhoneNumber": "22951010591", //Phone number you specify in your deposit request
    "currency": "XOF", //Currency of the transaction. 
    "amount": 2000, //Amount of the transaction
    "senderOperator": "MTN", // Mobile Money wallet operator
    "startTimestampInSecond": 1638087167, // Timestamp in second of whe you init the transaction
    "endTimestampInSecond": 1638087167 //Timestamp in second of when the transaction is finished
  }
  ```
</details>


<br/>

## Get Your balance

Get your Chipdeals account's balance
<!-- <iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C184%2C195%2C0%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=10px&dsblur=12px&wc=true&wa=true&pv=31px&ph=31px&ln=true&fl=1&fm=Fira+Code&fs=14px&lh=133%25&si=false&es=2x&wm=false&code=const%2520momo%2520%253D%2520require%28%27@chipdeals/momo-api%27%29%253B%250Amomo.setApiKey%28%27test_FOdigzgSopV8GZggZa89%27%29%253B%250A%250Amomo%250A%2520%2520.balance%28%29%250A%2520%2520.then%28%28balance%29%253D%253Econsole.log%28balance%29%29%250A"
  style="width: 862px; height: 260px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe> -->


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
    "amount": 1045,
    "currency": "XAF"
  },
]
```

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

# Callback

Call back allow to be notified in your code as soon as events happen about your transaction, such as a successful payment or a failed transaction.

When an event occurs, we'll call concerned callback function and will pass a single parametter containing the details about the event, the state of the transaction and the data associated with it.

<br/>

## Structure of callback data
All webhook payloads (except virtual card debits) follow the same basic structure:

* an `event` field describing the type of event
* a `data` object. The contents of this object will vary depending on the event, but typically it will contain details of the event, including:
    * a `reference` containing the ID of the transaction
    * a `statusMessageCode`, containing a specific code that identify an exact state of the transaction. [See all `statusMessageCode`](#Status-Message-Code) 
    * a `statusMessage`, cantaining an human undertandable descrption of the exact state of the transaction
    * transaction details

<br/>

## Callback data sample

### Collection callback data sample
```json
{
  "reference": "dd1e2d17-5c21-4964-b58d-198fd2aac150", 
  "statusMessageCode": 200,
  "statusMessage": "transaction successfully processed", 
  "senderPhoneNumber": "22951010591", //Phonenumeber you speciy in your collection request
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
  "statusMessageCode": 200, 
  "statusMessage": "success", 
  "recipientPhoneNumber": "22951010591", //Phone number you specify in your deposit request
  "currency": "XOF", //Currency of the transaction. 
  "amount": 2000, //Amount of the transaction
  "senderOperator": "MTN", // Mobile Money wallet operator
  "startTimestampInSecond": 1638087167, // Timestamp in second of whe you init the transaction
  "endTimestampInSecond": 1638087167 //Timestamp in second of when the transaction is finished
}
```

<br/>
<br/>


# More Info

## Collection and disburssement parametters

### - ``phoneNumber`` parameter

You specify the ``phoneNumber`` of a collection with method `from(phoneNumber: sting)`

And for a disbursement you specify ``phoneNumber`` with method `to(phoneNumber: sting)`

Those methode are locking for a string containing the phone number of the money sender for collection and the money recipient for disbursement.

The PhoneNumber should respect this format:

``XXXOOOOOOOO``

where the three ``X`` represent the country phone préfix (``229`` | ``237`` for example), and the ``O`` are the phone number in the country. The number of `O` can change with the country but the prefix should be 3.

### - ``currency`` parameter 
You specify the currenct of a transaction with the method `currency(currency: string)`

The currency should be a string of 3 character.

If you add a currency that is not the currency localy used in the country of the phone number of the transaction, we will convert the amount of the transaction into local currency.

Our conversion rate evoluate like the market.

### - ``amount`` parameter 
You specify the amount of a transaction with the method `amount(transactionAmount: number)`

It is a number. 

We will check if the currency that you specify for the transaction is the local currency of the phone number's country and then we will convert the amount into local currency before perform the transaction with the customer

### - ``fistName`` and ``lastName`` parameters 

You respectively specify ``firstName`` and ``lastName`` with methods ``firstName(senderFirstName: string)`` and ``lastName(senderLastName: string)``

Those parametters are required to perform secured collection request.

Disbursement doesn't need them.

If you make 3 request per day with unspesified ``firstName`` and | or ``lastName`` we will block the 4th and the other. We recommand you to specify ``fisrtName`` and ``lastName`` for all your collection request.

In sandbox test, you have no probleme with ``firstName`` and ``lastName`` specification, you can do as you like **But be careful for Live**

<br/>

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
