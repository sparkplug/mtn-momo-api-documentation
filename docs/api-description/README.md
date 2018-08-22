---
title: API Description
sidebarDepth: 2
---

# API Description

## Authentication

MTN API uses OAUTH 2.0 Authentication. Every API call is authenticated using the OAuth token. An API is available to generate the OAuth token. To generate a token a developer requires base64 encoded string of API key and API Secret. As usual a developer also requires the subscription key to access the Token API.

API key and Secret is generated and managed from the Merchants Partner Portal. In order to make an API call, you need to authenticate your app. There are two credentials used in the MTN Mobile Money Open API.

- Subscription Key
- API User and API Key for Oauth 2.0

The `Subscription Key` is used to give access to APIs in the API Manager portal. A user is assigned a subscription Key as and when the user subscribes to products in the API Manager Portal. 

The `API User` and `API Key` is used to grant access to the wallet system in a specific country. API user and Key is wholly managed by the merchant through Partner Portal. Merchants is allowed to generate/revoke API Keys through the Partner Portal.


### Subscription Key

The subscription key is part of the header of all requests sent to the Open API. The subscription key can be found in the user profile in the API Manager Portal. The subscription key is assigned to the Ocp-Apim-Subscription-Key parameter of the header.


### Oauth 2.0

The Open API is using Oauth 2.0 token for authentication of request. Client will request an access token using Client Credential Grant according to RFC 6749. The token received is according to RFC 6750 Bearer Token.

The `API User` and `API Key` are used in the basic authentication header when requesting the access token. The API user and key are managed in the Partner GUI for the country where the account is located. The Partner can create and manage API user and key from the Partner GUI. 

The received token has an expiry time. The same token can be used used for transactions until it is expired. A new token is requested by using the POST /token service in the same way as for the initial token. The new token can be requested before the previous has expired to avoid authentication failure due to expired token. 

::: warning 
The token must be treated as a credential and kept secret. The party that has access to the token will be authenticated as the user that requested the token.
:::
  
The below sequence describes the flow for requesting a token and using the token in a request. 

![Auth Sequence](/request-response.png) 

1.  Provider system request an access token using the API Key and API user as authentication. 
1.  Wallet platform authenticates credentials and respond with the access token
1.  Provider system will use the access token for any request that is sent to Wallet Platform e.g. `POST /requesttopay` 

Note: The same token shall be used if it is not expired.

## API Methods

The MTN Mobile Money API uses the POST, GET, PUT methods. This section gives an overview of the interaction sequence used in the API and the usage of the methods.

### POST

`POST` method is used for creating a resource in Wallet Platform. The request includes a reference id which is used to uniquely identify the specific resource that is created by the POST request. If a POST is using a reference id that is already used, then a duplication error response will be sent to the client.

Example: `POST /requesttopay`

`POST` is an asynchronous method. The Wallet Platform will validate the request to ensure that it is correct according to the API specification and then answer with HTTP 202 Accepted. The created resource will get status PENDING. Once the request has been processed the status will be updated to SUCCESSFUL or FAILED. The requester may then be notified of the final status through a callback.

### GET

`GET` is used for requesting information about a specific resource. The URL in the `GET` shall include the reference of the resource. If a resource was created with `POST` then the reference id that was provided in the request is used a the identity of the resource. 

Example: 

If  `POST /requesttopay` request is sent with `X-Reference-Id = 11377cbe-374c-43f6-a019-4fb70e57b617` then `GET /requesttopay/11377cbe-374c-43f6-a019-4fb70e57b617` will return the status of the request.

### PUT

The `PUT` method is used by the Open API when sending callbacks. Callback is sent if a callback URL is included in the `POST` request. The Wallet Platform will only send the callback once. There is no retry on the callback if the Partner system does not respond. If the callback is not received, then the Partner system can use GET to validate the status.


## Exploration Time

Now that we know how authentication works, and have a handle on the API methods, how about we explore use cases for the available API endpoints?

