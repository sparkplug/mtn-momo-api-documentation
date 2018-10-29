---
title: API Description
sidebarDepth: 2
---

# API Description

## Authentication

There are two credentials used in the Open API.

- Subscription Key
- API User and API Key for Oauth 2.0

The subscription key is used to give access to APIs in the API Manager portal. A user is assigned a subscription Key as and when the user subscribes to products in the API Manager Portal.

The API User and API Key are used to grant access to the wallet system in a specific country. API user and Key is wholly managed by the merchant through Partner Portal.

Merchants is allowed to generate/revoke API Keys through the Partner Portal.

However, on Sandbox Environment a Provisioning API is exposed to enable developers generate own API User and API Key for testing purposes only.

### Subscription Key
The subscription key is part of the header of all request sent to the API Manager. The subscription key can be found under user profile in the API Manager Portal.

The subscription key is assigned to the `Ocp-Apim-Subscription-Key` parameter the header.

### API User And API Key Management
The API user and API key are provisioned differently in the sandbox and production environment.

In the Sandbox a provisioning API is used to create the API User and API Key, where as in the production environment the provisioning is done through the Merchant Portal.

The sections below describe the different steps required in creating API User and API key in Sandbox and Production Environments.

## Sandbox Provisioning
The Steps below describes

## Create API User

<img :src="$withBase('/create-apiuser.png')" alt="createapiuser">

### Example

a) The Provider sends a POST /provisioning/v1_0/apiuser request to Wallet platform.

b) The Provider specifies the UUID Reference ID in the request Header and the subscription Key.

c) Reference ID will be used as the User ID for the API user to be created.

d) Wallet Platform creates the User and responds with 201

**Request:**

POST `https://pg-all.azure-api.net/provisioning/v1_0/apiuser HTTP/1.1`

Host: `pg-all.azure-api.net`

X-Reference-Id: `c72025f5-5cd1-4630-99e4-8ba4722fad56`

Ocp-Apim-Subscription-Key: `d484a1f0d34f4301916d0f2c9e9106a2`{"providerCallbackHost": "clinic.com"}

**Response:**

```201 Created```


## Create API Key

<img :src="$withBase('/create-apikey.png')" alt="createapikey">

a) The Provider sends a POST /provisioning/v1_0/apiuser/{APIUser}/apikey request to Wallet platform.

b) The Provider specifies the API User in the URL and subscription Key in the header.

c) Wallet Platform creates the API Key and responds with 201 Created with the newly Created API Key in the Body.

d) Provider now has both API User and API Key created.

### Example

**Request:**

POST `provisioning/v1_0/apiuser/c72025f5-5cd1-4630-99e4-8ba4722fad56/apikey HTTP/1.1`

Host: `pg-all.azure-api.net`

Ocp-Apim-Subscription-Key: `d484a1f0d34f4301916d0f2c9e9106a2`

**Response:**
```
HTTP/1.1 201 Created
date: Wed, 10 Oct 2018 09:16:15 GMT
content-type: application/json;charset=utf-8
content-length: 45
{
"apiKey": "f1db798c98df4bcf83b538175893bbf0"
}
```
### GET API User Details

Its possible to fetch API user details such as Call Back Host. However its not possible to fetch the API key.

Provider shall be required to generate a new Key should they lose the existing one.

<img :src="$withBase('/create-apiuser.png')" alt="createapuser">
