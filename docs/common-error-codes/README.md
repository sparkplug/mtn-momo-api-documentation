---
title: Common Error Codes
sidebarDepth: 2
---

## Common Error Codes

The complete definitions of error codes are found in the swagger documentation. Below is the list of error codes available.

Generic Error Codes

|  HTTP Code | Error Response Code   | Description |
| ------------- |-------------| -----|
| 409      | N/A | Duplicated Reference Id. Cannot create new recourse |
| 404      | N/A | 	Reference Id not found. Requested resource does not exist. |
| 400      | N/A | Bad request. Request does not follow the specification. |
| 401      | N/A | Authentication failed. Credentials not valid |
| 500      | NOT_ALLOWED | Authorization failed. User does not have permission. |
| 500      | 	NOT_ALLOWED_TARGET_ENVIRONMENT | Internal Error. |
| 500      | INVALID_CALLBACK_URL_HOST | 	Not allowed target environment |
| 500      | INVALID_CURRENCY | Callback URL with different host name then configured for API User |
| 500      | 	INTERNAL_PROCESSING_ERROR | 	Currency not supported on the requested account |
| 503      | SERVICE_UNAVAILABLE | Default error code used when there is no specific error mapping. |

## Preapproval Error Codes

|  HTTP Code | Error Response Code   | Description |
| ---------- |--------| ----- |
| 500      | PAYER_NOT_FOUND | Payer not found |

## RequestToPay Error Codes

|  HTTP Code | Error Response Code   | Description |
| ------------- |-------------| -----|
| 500      | 	PAYER_NOT_FOUND | 	Payer not found. Account holder is not registered. |
| 500      | PAYEE_NOT_ALLOWED_TO_RECEIVE |	Payee cannot receive funds due to e.g. transfer limit. |


## Transfer Error Codes

|  HTTP Code | Error Response Code   | Description |
| ------------- |-------------| -----|
| 500      | NOT_ENOUGH_FUNDS | Not enough funds on payer account |
| 500      | PAYER_LIMIT_REACHED | 	Not allowed to end due to Payer limit reached |
| 500      | PAYEE_NOT_FOUND | Payee not found. Account holder is not registered |

## Validate Account Holder Error Codes

|  HTTP Code | Error Response Code   | Description |
| ------------- |-------------| -----|
| 404      | 	N/A | Account holder is not found |
