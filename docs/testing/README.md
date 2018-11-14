---
title: Testing
sidebarDepth: 2
---

## Testing

To facilitate testing a set of predefined users and Test accounts are provided. These users and accounts have a predefined test scenario. A developer needs to [Signup](https://momodeveloper.mtn.com/signup) and [Subscribe](https://momodeveloper.mtn.com/products) to a Product before accessing any of the APIs.

The Sandbox URL is:
[https://momodeveloper.mtn.com/docs/services/collection/operations/requesttopay-POST](https://momodeveloper.mtn.com/docs/services/collection/operations/requesttopay-POST)



## OAuth Token

OAuth Token is generated from the merchants’ API Key and Secret. The `API Key` and `API Secret` can be obtained through the provisioning API in Sandbox, as described in the [API User and API Key Management](/api-description/#sandbox-provisioning) section.

## Target Environment

The Target Environment used in Testing is “sandbox”

## Test Currency

The currency used in Sandbox is `EUR`

## Test Numbers

The following Numbers are predefined with respective response for all Testcases

|  Number | Response |
| ------------- |-------------|
| 46733123450      | Failed |
| 46733123451      | Rejected |
| 46733123452      | Timeout |
| 46733123453     |  Ongoing (will answer pending first and if requested again after 30 seconds it will respond success)|
| 46733123454     | Pending |
| Any Other Number | Success |
