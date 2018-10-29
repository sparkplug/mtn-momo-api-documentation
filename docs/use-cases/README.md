---
title: Use Cases
sidebarDepth: 2
---

# Use Cases

This section describes the services in the Open API.

**Note: It’s expected that the developer has an access token as described in 2.2.2**

## Request to Pay

Request to Pay service is used for requesting a payment from a customer (Payer). This can be used by e.g. an online web shop to request a payment for a customer. The customer is requested to approve the transaction on the customer client.

<img :src="$withBase('/request-to-pay.png')" alt="request-to-pay">

a) Customer (Payer) have selected product(s) in the merchant web shop and decided to check out. Customer select to pay with Mobile Money.

b) The provider system collects the account information for the customer e.g. mobile number and calculate the total amount of the products.

c) The provider system sends a request to pay (POST /requesttopay) operation to Wallet Platform. This request includes the amount and customer (Payer) account holder number.

d) Wallet Platform will respond with HTTP 202 Accepted to the provider system

e) Provider shall inform the customer that a payment needs to be approved, by giving information on the merchant web page. For example, the merchant  could show information that payment is being processed and that customer needs to approve using the own client, e.g. USSD, mobile app.

f) Wallet Platform will process the request so that the customer can approve the payment. The request to pay will be in PENDING state until the customer have approved/Rejected the payment.

g) The Customer (Payer) will use his/her own client to review the payment. Customer can approve or reject the payment.

h) Wallet platform will transfer the funds if the customer approves the payment. Status of the payment is updated to SUCCESSFUL or FAILED.

i) If a callback URL was provided in the POST /requesttopay then a callback will be sent once the request to pay have reached a final state (SUCCESSFUL, FAILED). Note the callback will only be sent once. There is no retry.

j) GET request can be used for validating the status of the transaction. GET is used if the partner system has not requested a callback by providing a callback URL or if the callback was not received.

## Pre-Approval

Pre-approval is used to setup an auto debit towards a customer. The Partner can request a pre-approval from the customer. Once the customer has approved then the partner can debit the customer account without authorization from the customer.

The call flow for setting up a pre-approval is like the request to pay use case. The following picture describes the sequence for pre-approval.

<img :src="$withBase('/preapproval.png')" alt="preapproval">

a) The Provider sends a POST /preapproval request to Wallet platform.

b) Provider shall inform the customer that pre-approval needs to be approved.

c) Customer (Payer) will use the own client to view the pre-approval request. Customer can approve or reject the request.

d) Callback will be sent if a callback URL was provided in the POST request. The callback is sent when the request has reach a final state (Successful, Failed).

e) The Provider can use the GET request to validate the status of the pre-approval.

## Transfer

Transfer is used for transferring money from the provider account to a customer.

The below sequence gives an overview of the flow of the transfer use case.

<img :src="$withBase('/transfer.png')" alt="transfer">

a) The Provider sends a POST /transfer request to Wallet platform.

b) Wallet platform will directly respond to indicate that the request is received and will be processed.

c) Wallet platform will authorize the request to ensure that the transfer is allowed. The funds will be transferred from the provider account to the d) Payee account provided in the transfer request.

e) Callback will be sent if a callback URL was provided in the POST request. The callback is sent when the request has reach a final state (SUCCESSFUL, FAILED).

f) The Provider can use the GET request to validate the status of the transfer.

## Validate Account Holder

Validate account holder can be used to do a validation if a customer is active and able to receive funds. The use case will only validate that the customer is available and active. It does not validate that a specific amount can be received.

The sequence for the validate account holder is described below.

<img :src="$withBase('/validate-account.png')" alt="validate-account">

a) The Partner can send a `GET /accountholder` request to validate is a customer is active. The Partner provides the id of that customer as part of the URL

b) Wallet platform will respond with `HTTP 200` if the account holder is active.

## Get Balance

Get balance request is used to check the balance on the default account connected to the API User. The following is the sequence flow for get balance use case.

<img :src="$withBase('/get-balance.png')" alt="get-balance">


a) The partner will send a GET /account/balance request

b) Wallet platform will respond with the available balance on the API user account.



































<!--

This section describes the different services in the Open API. It is expected that a developer has an access token by this point.

## Request to Pay

Request to Pay service is used for requesting a payment from a customer (Payer). This can be used by e.g. an online web shop to request a payment for a customer. The customer is requested to approve the transaction on the customer client.


### How it works

This operation is used to request a payment from a customer (Payer). The payer will be asked to authorize the payment. The transaction will be executed once the payer have authorized the payment. An example use case is an online web shop requesting a payment from a customer at checkout. The requesttopay will be in status PENDING until the transaction is authorized, declined by the payer or it is timed out by the system. Status of the transaction can validated by using the `GET /requesttopay/<resourceId>`.

The sequence below describes how the `requesttopay` service is used:

![Request to Pay](/request-to-pay.png)

1.	Customer has selected product(s) in the merchant web shop and decided to check out. Customer select to pay with Mobile Money.
2.	The provider system collects the account information for the customer e.g. mobile number and calculate the total amount of the products.
3.	The provider system sends a request to pay (POST /requesttopay) operation to Wallet Platform. This request includes the amount and customer account holder number.
4.	Wallet Platform will respond with HTTP 202 Accepted to the provider system.
5.	Provider shall inform the customer that a payment needs to be approved, by giving information on the merchant web page. For example, the merchant could show information that payment is being processed and that customer needs to approve using the own client, e.g. USSD or mobile app.
6.	Wallet Platform will process the request so that the customer can approve the payment. The request to pay will be in PENDING state until the customer has approved or rejected the payment.
7.	The Customer will use her own client to review the payment and can approve or reject the payment.
8.	Wallet platform will transfer the funds if the customer approves the payment. Status of the payment is updated to SUCCESSFUL or FAILED.
9.	If a callback URL was provided in the `POST /requesttopay` then a callback will be sent once the request to pay has reached a final state
(SUCCESSFUL, FAILED).
::: warning
Note the callback will only be sent once. There is no retry.
:::
10.	`GET` request can be used for validating the status of the transaction. `GET` is used if the partner system has not requested a callback by providing a callback URL or if the callback was not received.

### Request to Pay POST URL

`POST https://pg-all.azure-api.net/testpg/v1_0/requesttopay`

What might the Request Body look like? Here is a sample:

```json
{
  "amount": "string",
  "currency": "string",
  "externalId": "string",
  "payer": {
    "partyIdType": "MSISDN",
    "partyId": "string"
  },
  "payerMessage": "string",
  "payeeNote": "string"
}
```

Below is a description of what each property means

| Property        | Type           | Description |
| ------------- |:-------------:| :-----|
| amount      | string | Amount that will be debited from the payer account. |
| currency      | string      |   ISO4217 Currency |
| externalId | string     |    External id is used as a reference to the transaction. External id is used for reconciliation. The external id will be included in transaction history report. <br>External id is not required to be unique. |
| payer      | object | Party identifies a account holder in the wallet platform. Party consists of two parameters, type and partyId. Each type have its own validation of the partyId<br><br> MSISDN - Mobile Number validated according to ITU-T E.164. Validated with IsMSISDN<br> EMAIL - Validated to be a valid e-mail format. Validated with IsEmail<br> PARTY_CODE - UUID of the party. Validated with IsUuid |
| partyIdType      | string      |   "enum": ["MSISDN", "EMAIL", "PARTY_CODE"] |
| partyId      | string |  |
| payerMessage      | string      |   Message that will be written in the payer transaction history message field. |
| payerNote      | string      |   Message that will be written in the payee transaction history note field. |

An example code sample for this API call using Curl would look something like this:

```bash

curl -v -X POST "https://pg-all.azure-api.net/testpg/v1_0/requesttopay"
-H "Authorization: "
-H "X-Callback-Url: "
-H "X-Reference-Id: "
-H "X-Target-Environment: "
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: {subscription key}"

--data-ascii "{body}"
```

Same call in Python 2.7 would look like this:

```python
########### Python 2.7 #############
import httplib, urllib, base64

headers = {
    # Request headers
    'Authorization': '',
    'X-Callback-Url': '',
    'X-Reference-Id': '',
    'X-Target-Environment': '',
    'Content-Type': 'application/json',
    'Ocp-Apim-Subscription-Key': '{subscription key}',
}

params = urllib.urlencode({
})

try:
    conn = httplib.HTTPSConnection('pg-all.azure-api.net')
    conn.request("POST", "/testpg/v1_0/requesttopay?%s" % params, "{body}", headers)
    response = conn.getresponse()
    data = response.read()
    print(data)
    conn.close()
except Exception as e:
    print("[Errno {0}] {1}".format(e.errno, e.strerror))

####################################

########### Python 3.2 #############
import http.client, urllib.request, urllib.parse, urllib.error, base64

headers = {
    # Request headers
    'Authorization': '',
    'X-Callback-Url': '',
    'X-Reference-Id': '',
    'X-Target-Environment': '',
    'Content-Type': 'application/json',
    'Ocp-Apim-Subscription-Key': '{subscription key}',
}

params = urllib.parse.urlencode({
})

try:
    conn = http.client.HTTPSConnection('pg-all.azure-api.net')
    conn.request("POST", "/testpg/v1_0/requesttopay?%s" % params, "{body}", headers)
    response = conn.getresponse()
    data = response.read()
    print(data)
    conn.close()
except Exception as e:
    print("[Errno {0}] {1}".format(e.errno, e.strerror))

####################################
```

The API then then sends a `Response` to the request that has a numeric code, string text and a message.

|  Code        | String           | Message |
| ------------- |:-------------| :-----|
| 200      | Accepted | Accepted |
| 400      | Bad Request | Bad request, e.g. invalid data was sent in the request. |
| 409      | Conflict | Conflict, duplicated reference id |
| 500      | Internal Server Error | Internal Error. |

The actual error messages for 409 and 500 are a little more nuanced than shown. The schema for the enum of the optional messages looks something like this:

```json
{
  "type": "object",
  "properties": {
    "code": {
      "type": "string",
      "enum": [
        "PAYEE_NOT_FOUND",
        "PAYER_NOT_FOUND",
        "NOT_ALLOWED",
        "NOT_ALLOWED_TARGET_ENVIRONMENT",
        "INVALID_CALLBACK_URL_HOST",
        "INVALID_CURRENCY",
        "SERVICE_UNAVAILABLE",
        "INTERNAL_PROCESSING_ERROR",
        "NOT_ENOUGH_FUNDS",
        "PAYER_LIMIT_REACHED",
        "PAYEE_NOT_ALLOWED_TO_RECEIVE",
        "PAYMENT_NOT_APPROVED",
        "RESOURCE_NOT_FOUND",
        "APPROVAL_REJECTED",
        "EXPIRED",
        "TRANSACTION_CANCELED",
        "RESOURCE_ALREADY_EXIST"
      ]
    },
    "message": {
      "type": "string"
    }
  }
}
```

This derives a similar response for the `GET` request.

### Request to Pay GET URL

`GET https://pg-all.azure-api.net/testpg/v1_0/requesttopay/{referenceId}`



## Pre-Approval

The preApproval service is used to request a pre approval for requestToPay operations so that the payer do not need to approval the requestToPay operation.

### How it works

Pre-approval is used to setup an auto debit towards a customer. The Partner can request a pre-approval from the customer. Once the customer has approved then the partner can debit the customer account without authorization from the customer. The call flow for setting up a pre-approval is like the request to pay use case.

The diagram below describes the sequence for pre-approval.

![Pre-approval](/request-to-pay.png)

1.	The Provider sends a POST /preapproval request to Wallet platform.
2.	Provider shall inform the customer that pre-approval needs to be approved.
3.	Customer (Payer) will use the own client to view the pre-approval request. Customer can approve or reject the request.
4.	Callback will be sent if a callback URL was provided in the POST request. The callback is sent when the request has reach a final state (Successful, Failed).
5.	The Provider can use the GET request to validate the status of the pre-approval.

The request body schema looks something like this:

```json
{
  "type": "object",
  "properties": {
    "payer": {
      "type": "object",
      "description": "Party identifies a account holder in the wallet platform. Party consists of two parameters, type and partyId. Each type have its own validation of the partyId<br> MSISDN - Mobile Number validated according to ITU-T E.164. Validated with IsMSISDN<br> EMAIL - Validated to be a valid e-mail format. Validated with IsEmail<br> PARTY_CODE - UUID of the party. Validated with IsUuid",
      "properties": {
        "partyIdType": {
          "type": "string",
          "enum": [
            "MSISDN",
            "EMAIL",
            "PARTY_CODE"
          ]
        },
        "partyId": {
          "type": "string"
        }
      }
    },
    "payerCurrency": {
      "type": "string",
      "description": "ISO4217 Currency"
    },
    "payerMessage": {
      "type": "string",
      "description": "The mesage that is shown to the approver."
    },
    "validityTime": {
      "type": "integer",
      "description": "The request validity time of the pre-approval"
    }
  }
}
```

An example code sample for the API call using Curl would look like this:

```bash
curl -v -X POST "https://pg-all.azure-api.net/testpg/v1_0/preapproval"
-H "Authorization: "
-H "X-Callback-Url: "
-H "X-Reference-Id: "
-H "X-Target-Environment: "
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: {subscription key}"

--data-ascii "{body}"
```

Here is a sample of the `Request Body` for that call:

```json
{
  "payer": {
    "partyIdType": "MSISDN",
    "partyId": "string"
  },
  "payerCurrency": "string",
  "payerMessage": "string",
  "validityTime": 0
}
```

The API then then sends a `Response` to the request that has a numeric code, string text and a message.

|  Code        | String           | Message |
| ------------- |:-------------| :-----|
| 200      | Accepted | Accepted |
| 400      | Bad Request | Bad request, e.g. invalid data was sent in the request. |
| 409      | Conflict | Conflict, duplicated reference id |
| 500      | Internal Server Error | Internal Error. |

### Request URL for `GET` and `POST`

`https://pg-all.azure-api.net/testpg/v1_0/preapproval`


## Transfer

Transfer operation is used to transfer an amount from the own account to a payee account. An example is when a betting house needs to pay out the winnings of a customer. Status of the transaction can validated by using the GET /transfer/{referenceId}


### How it works

The below sequence gives an overview of the flow of the transfer use case.

![Transfer](/transfer.png)

What actually happens here? Let's look at the steps:

1.	The Provider sends a POST /transfer request to Wallet platform.
2.	Wallet platform will directly respond to indicate that the request is received and will be processed.
3.	Wallet platform will authorize the request to ensure that the transfer is allowed. The funds will be transferred from the provider account to the Payee account provided in the transfer request.
4.	Callback will be sent if a callback URL was provided in the POST request. The callback is sent when the request has reach a final state (SUCCESSFUL, FAILED).
5.	The Provider can use the GET request to validate the status of the transfer.


Code sample for the request with Curl looks like this:

```bash
curl -v -X POST "https://pg-all.azure-api.net/testpg/v1_0/transfer"
-H "Authorization: "
-H "X-Callback-Url: "
-H "X-Reference-Id: "
-H "X-Target-Environment: "
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: {subscription key}"

--data-ascii "{body}"
```

The sample request body for it:

```json
{
  "amount": "string",
  "currency": "string",
  "externalId": "string",
  "payee": {
    "partyIdType": "MSISDN",
    "partyId": "string"
  },
  "payerMessage": "string",
  "payeeNote": "string"
}
```

The API then then sends a `Response` to the request that has a numeric code, string text and a message.

|  Code        | String           | Message |
| ------------- |:-------------| :-----|
| 200      | Accepted | Accepted |
| 400      | Bad Request | Bad request, e.g. invalid data was sent in the request. |
| 409      | Conflict | Conflict, duplicated reference id |
| 500      | Internal Server Error | Internal Error. |

### Request URL for `GET` and `POST`

`https://pg-all.azure-api.net/testpg/v1_0/transfer`


## Validate Account Holder

This operation is used to check if an account holder is registered and active in the system. Validate account holder can be used to do a validation if a customer is active and able to receive funds. The use case will only validate that the customer is available and active. It does not validate that a specific amount can be received.

### How It Works

The sequence for the validate account holder is described below:

![Validate Account](/validate-account.png)

1.	The Partner can send a GET /accountholder request to validate is a customer is active. The Partner provides the id of that customer as part of the URL
2.	Wallet platform will respond with HTTP 200 if the account holder is active.

::: tip
Please note that this API call only supports `GET` requests. :smile:
:::

Curl sample code to illustrate how this might work:

```bash
curl -v -X GET "https://pg-all.azure-api.net/testpg/v1_0/accountholder/{accountHolderIdType}/{accountHolderId}/active"
-H "Authorization: "
-H "X-Target-Environment: "
-H "Ocp-Apim-Subscription-Key: {subscription key}"

--data-ascii "{body}"
```


### Request URL

`https://pg-all.azure-api.net/testpg/v1_0/accountholder/{accountHolderIdType}/{accountHolderId}/active`



## Get Balance

This operation enables you to get the balance of a user account

### How It Works

Get balance request is used to check the balance on the default account connected to the API User. The diagram below is the sequence flow for the `Get Balance` use case.

![Get Balance](/get-balance.png)

1.	The partner will send a GET /account/balance request
2.	Wallet platform will respond with the available balance on the API user account.

::: tip
Please note that this API call only supports `GET` requests. :smile:
:::

Sample code to show this call might work with Curl:

```bash
curl -v -X GET "https://pg-all.azure-api.net/testpg/v1_0/account/balance"
-H "Authorization: "
-H "X-Target-Environment: "
-H "Ocp-Apim-Subscription-Key: {subscription key}"

--data-ascii "{body}"
```

### Request URL

`https://pg-all.azure-api.net/testpg/v1_0/account/balance`

-->
