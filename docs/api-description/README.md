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

## Subscription Key
The subscription key is part of the header of all request sent to the API Manager. The subscription key can be found under user profile in the API Manager Portal.

The subscription key is assigned to the `Ocp-Apim-Subscription-Key` parameter the header.

## API User And API Key Management
The API user and API key are provisioned differently in the sandbox and production environment.

In the Sandbox a provisioning API is used to create the API User and API Key, where as in the production environment the provisioning is done through the Merchant Portal.

The sections below describe the different steps required in creating API User and API key in Sandbox and Production Environments.

## Sandbox Provisioning
The Steps below describes

## Create API User
