---
title: Testing
sidebarDepth: 2
---

# Testing

To facilitate testing of the API, a set of predefined users and test accounts are provided. These users and accounts have a predefined test scenario in a Sandbox. The Sandbox URL is: https://pg-all.portal.azure-api.net/ A developer needs to signup and subscribe to the Starter Product before accessing any of the APIs.

## Oauth Token

The Oauth token is generated from the merchants’ API Key and Secret. The Sandbox has one merchant defined with `API Key` and `API Secret` parameters:

**API Key** : c906211b-8f92-48b5-a9d3-c28ef023aa7a

**API Secret** : 151ac58ddcae42c689bcdcb7281d962c

## Target Environment

The target environment used for testing is “sandbox". 

::: tip
The currency used in Sandbox is EUR
:::


## Test Numbers

There is a specific set of numbers used in the sandbox, and each one returns a specific response for all test cases. Below is a table of the numbers.

| Number        | Response     | 
| ------------- |:-------------| 
| 46733123450   | failed       | 
| 46733123451   | rejected       | 
| 46733123452   | timeout       | 
| 46733123453   | ongoing (will answer pending first and if requested again after 30 seconds it will respond success)       | 
| 46733123454   | pending       | 
