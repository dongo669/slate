--- 

title: Wallet to ATOMIC APIs 

language_tabs: 
   - shell 

toc_footers: 
   - <a href='#'>Sign Up for a Developer Key</a> 
   - <a href='https://github.com/lavkumarv'>Documentation Powered by lav</a> 

includes: 
   - errors 

search: true 

--- 

# Introduction 

APIs from an ATOMIC powered wallet towards ATOMIC P2P collateral guarantees service 

**Version:** 1.0 

# /API/V1/USERS/{USER-ID}
## ***GET*** 

**Summary:** Get user's information

**Description:** Retrieves existing information for a specific user

### HTTP Request 
`***GET*** /api/v1/users/{user-id}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| user-id | path | User ID | Yes | string (uuid) |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | user found |
| 404 | user not found |

## ***PUT*** 

**Summary:** Create new user

**Description:** Register user with ATOMIC - creating a smart-credit address

### HTTP Request 
`***PUT*** /api/v1/users/{user-id}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| user-id | path | User ID | Yes | string (uuid) |
| registerUserParams | body | Parameters for user registeration | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 201 | user registered |
| 400 | invalid input, object invalid |
| 409 | user already exists |

# /API/V1/USERS/{USER-ID}/UNLOCK
## ***GET*** 

**Summary:** Get on-demand unlock

**Description:** Get an on-demand unlock transaction from the user's smart credit address


### HTTP Request 
`***GET*** /api/v1/users/{user-id}/unlock` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| user-id | path | User ID | Yes | string (uuid) |
| getUnlockParams | body | Parameters for getting an unlock transaction | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | transaction created |
| 400 | invalid input, object invalid |
| 403 | cannot perform unlock at this time |
| 404 | user not found |

## ***PUT*** 

**Summary:** Submit an on-demand unlock

**Description:** Perform an unlock of funds from the user's smart-credit.
Should supply a signed unlock transaction which was obtained from the "GET" command, or created manually


### HTTP Request 
`***PUT*** /api/v1/users/{user-id}/unlock` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| user-id | path | User ID | Yes | string (uuid) |
| performUnlockParams | body | Parameters for performing the unlock command | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 201 | unlock command recieved |
| 400 | invalid input, object invalid |
| 403 | cannot perform unlock, another unlock/transaction is already in progress |
| 404 | user not found |
| 409 | already exists |

# /API/V1/USERS/{USER-ID}/UNLOCK/TIMED
## ***GET*** 

**Summary:** Get timed unlock

**Description:** Get a timed unlock transaction from the user's smart credit address at the given lock time


### HTTP Request 
`***GET*** /api/v1/users/{user-id}/unlock/timed` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| user-id | path | User ID | Yes | string (uuid) |
| getTimedUnlockParams | body | Parameters for getting a timed unlock transaction | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | transaction created |
| 400 | invalid input, object invalid |
| 403 | cannot perform the requested timed unlock |
| 404 | user not found |

## ***PUT*** 

**Summary:** Submit timed unlock

**Description:** Requests ATOMIC to sign the user-signed timed unlock transaction.
The timed transaction will be stored by the user, and submitted when the lock time arrives


### HTTP Request 
`***PUT*** /api/v1/users/{user-id}/unlock/timed` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| user-id | path | User ID | Yes | string (uuid) |
| performUnlockParams | body | Parameters for performing the unlock command | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 201 | unlock command received |
| 400 | invalid input, object invalid |
| 403 | cannot perform unlock, earlier unlock/transaction is already in progress |
| 404 | user not found |
| 409 | already exists |

# /API/V1/USERS/{USER-ID}/PAYMENTS/{PAYMENT-ID}
## ***GET*** 

**Summary:** Get payment details

**Description:** Gets the information needed to confirm an order.
ATOMIC will provide the raw escrow transaction, as well as the required details for the payment


### HTTP Request 
`***GET*** /api/v1/users/{user-id}/payments/{payment-id}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| user-id | path | User ID | Yes | string (uuid) |
| payment-id | path | Payment ID | Yes | string (uuid) |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | payment found |
| 400 | invalid input, object invalid |
| 404 | user/payment not found |
| 409 | payment already performed |

## ***PUT*** 

**Summary:** Confirm payment

**Description:** Confirms the payment.
Providing ATOMIC with the signed transactions which were obtained from the "GET" command


### HTTP Request 
`***PUT*** /api/v1/users/{user-id}/payments/{payment-id}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| user-id | path | User ID | Yes | string (uuid) |
| payment-id | path | Payment ID | Yes | string (uuid) |
| putPaymentParams | body | Parameters for performing the payment | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 201 | payment received |
| 400 | invalid input, object invalid |
| 404 | user/payment not found |
| 409 | payment already performed |

# /API/V1/INFO
## ***GET*** 

**Summary:** Get ATOMIC generic information

**Description:** Get general ATOMIC information


### HTTP Request 
`***GET*** /api/v1/info` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | OK |
| 400 | bad input parameter |

<!-- Converted with the swagger-to-slate https://github.com/lavkumarv/swagger-to-slate -->
