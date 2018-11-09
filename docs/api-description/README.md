---
title: API User & API Key Management
sidebarDepth: 3
---

# API User and API Key Management

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

### Create API User

<img :src="$withBase('/create_apiuser.png')" alt="Create API User">

a) The Provider sends a POST /provisioning/v1_0/apiuser request to Wallet platform.

b) The Provider specifies the UUID Reference ID in the request Header and the subscription Key.

c) Reference ID will be used as the User ID for the API user to be created.

d) Wallet Platform creates the User and responds with 201

### Example

**Request:**

POST `https://momodeveloper.mtn.com/v1_0/apiuser HTTP/1.1`

Host: `momodeveloper.mtn.com`

X-Reference-Id: `c72025f5-5cd1-4630-99e4-8ba4722fad56`

Ocp-Apim-Subscription-Key: `d484a1f0d34f4301916d0f2c9e9106a2`{"providerCallbackHost": "clinic.com"}

**Response:**

```201 Created```


## Create API Key

<img :src="$withBase('/create_apikey.png')" alt="Create API Key">

a) The Provider sends a POST /provisioning/v1_0/apiuser/{APIUser}/apikey request to Wallet platform.

b) The Provider specifies the API User in the URL and subscription Key in the header.

c) Wallet Platform creates the API Key and responds with 201 Created with the newly Created API Key in the Body.

d) Provider now has both API User and API Key created.

### Example

**Request:**

POST `/v1_0/apiuser/c72025f5-5cd1-4630-99e4-8ba4722fad56/apikey HTTP/1.1`

Host: `momodeveloper.mtn.com`

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

<img :src="$withBase('/get_apiuser_details.png')" alt="Get API User Details">

a) The Provider sends a GET /provisioning/v1_0/apiuser/{APIUser} request to Wallet platform.

b) The Provider specifies the API User in the URL and subscription Key in the header.

c) Wallet Platform responds with 200 Ok and details of the user.

d) TargetEnvironment is preconfigured to sandbox in the Sandbox environment, therefore Providers will not have the option of setting it to a different parameter.

### Example

GET `/v1_0/apiuser/ c72025f5-5cd1-4630-99e4-8ba4722fad56`

Host: `momodeveloper.mtn.com`

Ocp-Apim-Subscription-Key: `d484a1f0d34f4301916d0f2c9e9106a2`

**Response:**
```
HTTP/1.1 200 Accepted
date: Wed, 10 Oct 2018 09:16:15 GMT
{
"providerCallbackHost": "clinic.com",
"targetEnvironment": "sandbox"
}
```

## Production Provisioning

Production API User and API Key are provisioned and managed on the Partner Portal

Partner Portal is the wallet portal granted to partners after Go-Live. The credentials for this portal are shared with partners after Go-live.

### Log on to Partner Portal

The URL and Credentials for partner portal are to be obtained after Go-live

<img :src="$withBase('/logon-partnerportal.png')" alt="logon-partnerportal">

### Create API user

Partner shall Click on the Top right under user profile to access the option.

Partner shall click on the Create API user Option shown below.

<img :src="$withBase('/create-apiuser1.png')" alt="create-apiuser1">

Partner shall fill the in the callback URL and select a transaction wallet.

<img :src="$withBase('/portal_create_apiuser.png')" alt="create-apiuser2">

The table below describes the different fields required when creating an API User

| Field         | Optionality   | Description   |
| ------------- | ------------- | ------------- |
| Account       | Mandatory     | Drop down option with available wallet. Partner shall choose main Transaction Wallet |
| Provider Callback Host  | Mandatory  | Partner Subdomain.Example `www.myshop.com` |
| Payment Server URL  | Mandatory   | This shall be the callback url Example: `https://myshop.com/payments`  |
| Gateway URL  | Optional | This should be left empty. |

Partner shall click Ok after filling all the mandatory fields.

Upon submission ECW creates the API User and API key. API Key is displayed as a flash message as shown below.

<img :src="$withBase('/create-apiuser3.png')" alt="create-apiuser3">

Its also possible for partner to delete and re-cerate an API User.

## Oauth 2.0

The Open API is using Oauth 2.0 token for authentication of request. Client will request an access token using Client Credential Grant according to RFC 6749. The token received is according to RFC 6750 Bearer Token.

The API user and API key are used in the basic authentication header when requesting the access token. The API user and key are managed in the Partner GUI for the country where the account is located. The Partner can create and manage API user and key from the Partner GUI.

In case of sandbox the API Key and API User are managed through a Provisioning API as described on 3.2.2

The received token have an expiry time. The token same token can be used used for transactions until it is expired. A new token is requested by using the `POST` /token service in the same way as for the initial token. The new token can be requested before the previous have expired to avoid authentication failure due to expired token.

> **Important:**
 The token must be treated as a credential and kept secret. The party that have access to the token will be authenticated as the user that requested the token.
 The below sequence describes the flow for requesting a token and using the token in a request.

 <img :src="$withBase('/oauth_img.png')" alt="oauth_img">

a) Provider system request an access token using the API Key and API user as authentication.
b) Wallet platform authenticates credentials and respond with the access token
3) Provider system will use the access token for any request that is sent to Wallet Platform, e.g. `POST /requesttopay`

**Note: The same token shall be used if it is not expired.**

## API Methods

The API is using POST, GET, PUT methods. This section gives an overview of the interaction sequence used in the API and the usage of the methods.

### POST

POSTmethod is used for creating a resource in Wallet Platform. The request includes a reference id which is used to uniquely identify the specific resource that are created by the POST request. If a POST is using a reference id that is already used, then a duplication error response will be sent to the client.

Example: `POST /requesttopay`

The `POST` is an asynchronous method. The Wallet Platform will validate the request to ensure that it correct according to the API specification and then answer with HTTP 202 Accepted. The created resource will get status PENDING. Once the request has been processed the status will be updated to SUCCESSFUL or FAILED. The requester may then be notified of the final status through callback as described in 4.1

### GET

GET is used for requesting information about a specific resource. The URL in the GET shall include the reference of the resource. If a resource was created with POST then the reference id that was provided in the request is used a the identity of the resource.

Example:

`POST /requesttopay` request is sent with X-Reference-Id = `11377cbe-374c-43f6-a019-4fb70e57b617`

`GET /requesttopay/11377cbe-374c-43f6-a019-4fb70e57b617` will return the status of the request.

### PUT

The `PUT` method is used by the Open API when sending callbacks. Callback is sent if a callback URL is included in the `POST` request. The Wallet Platform will only send the callback once. There is no retry on the callback if the Partner system does not respond. If the callback is not received, then the Partner system can use GET to validate the status.


## Onto the use cases

Now we are ready to take a look at our [Use Cases](/use-cases/) to see how you can use our API.