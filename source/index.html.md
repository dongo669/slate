---
title: Wallet to ATOMIC APIs
language_tabs:
  - shell: cURL
toc_footers:
- <a href='https://www.atomic.org'>Visit ATOMIC home page</a>
includes:
  []
search: true
highlight_theme: darkula
headingLevel: 2

---

<!-- Generator: Widdershins v3.6.6 -->

# Introduction

> Scroll down for code samples, example requests and responses

This page details the APIs from an ATOMIC powered wallet towards ATOMIC P2P collateral guarantees service


# User Management

User management related APIs

## Create new user


Register user with ATOMIC - creating a Private Collateral address


`PUT /api/v1/users/{user-id}`

> Body parameters

```json
{
    "public-key": "027a478fd4a7c3b8e43cfd2eaf0e439158b2b874dace2b598c585e88f67eeb8655",
    "refund-address": "2NF1qc9L4j1tyfqzzLdRzRqoVftHznSeC6K"
}
```

> Code samples

```shell
curl -request PUT https://atomic.org/wallet-to-atomic/api/v1/users/{user-id} 
--data '{"public-key": "027a478fd4a7c3b8e43cfd2eaf0e439158b2b874dace2b598c585e88f67eeb8655", "refund-address": "2NF1qc9L4j1tyfqzzLdRzRqoVftHznSeC6K"}'
```

<!-- 
```python
import requests
headers = {
'Content-Type': 'application/json',
'Accept': 'application/json'
}

r = requests.put('https://virtserver.swaggerhub.com/Atomicbox/Wallet-to-ATOMIC-APIs/1.0/api/v1/users/{user-id}', params={

}, headers = headers)

print r.json()

``` -->


> Example responses

> 201 Response

```json
{
    "private-collateral-info": {
    "atomic-public-key": "022d4a565aa6845c72bd2a732d3d4971704e8403942766b59d68e0dcf62713443c",
    "private-collateral-address": "2Mx1EM2Bsd8qzV2yFv3XxemKaqW7ZfCKAFN",
    "private-collateral-redeem-script": "5221022d4a565aa6845c72bd2a732d3d4971704e8403942766b59d68e0dcf62713443c21027a478fd4a7c3b8e43cfd2eaf0e439158b2b874dace2b598c585e88f67eeb865552ae",
    "private-collateral-script-pub-key": "a91434331a9a1282cb1f58a383eb3d74bbbe671161ba87"
    }
}
```
### Return Codes
Status|Meaning|Description
---|---|---
201 | OK  | User created succesfully
400 | Bad Request | Invalid request or conflict with existing user
409 | Already Exist | User already exists

## Get user's information
Retrieves existing information for a specific user

`GET /api/v1/users/{user-id}`

> Code samples

```shell
curl -request GET https://atomic.org/wallet-to-atomic/api/v1/users/{user-id}
```
<!--
```python
import requests
headers = {
'Accept': 'application/json'
}

r = requests.get('https://virtserver.swaggerhub.com/Atomicbox/Wallet-to-ATOMIC-APIs/1.0/api/v1/users/{user-id}', params={

}, headers = headers)

print r.json()

``` -->

> Example responses

> 200 Response

```json
{
  "private-collateral-info": {
    "atomic-public-key": "022d4a565aa6845c72bd2a732d3d4971704e8403942766b59d68e0dcf62713443c",
    "private-collateral-address": "2Mx1EM2Bsd8qzV2yFv3XxemKaqW7ZfCKAFN",
    "private-collateral-redeem-script": "5221022d4a565aa6845c72bd2a732d3d4971704e8403942766b59d68e0dcf62713443c21027a478fd4a7c3b8e43cfd2eaf0e439158b2b874dace2b598c585e88f67eeb865552ae",
    "private-collateral-script-pub-key": "a91434331a9a1282cb1f58a383eb3d74bbbe671161ba87"
  },
  "private-collateral-balance": 102340000,
  "inflight-transactions": [
    {
      "payment-id": "d290f1ee-6c54-4b01-90e6-d701748f0300",
      "transaction-id": "850b5f43b0b13837f2fea92818c73034c7337c963245eaa4eca8b78e00515fd0",
      "status": "PENDING-CONFIRMATIONS"
    }
  ]
}
```
### Return Codes
Status|Meaning|Description
---|---|---
200 | OK | OK
404 | Not Found | User not found

# Unlock Operations

## Get on-demand unlock
Get an on-demand unlock transaction from the user's private-collateral address

`GET /api/v1/users/{user-id}/unlock`

<aside class="warning">
NOTE: An on demand unlock cannot be processed during ongoing transactions
</aside>


> Body parameters

```json
{
    "amount": 498000000
}
```

> Code samples

```shell
curl -request GET https://atomic.org/wallet-to-atomic/api/v1/users/{user-id}/unlock 
--data '{"amount": 498000000 }'
```
<!--
```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json'
}

r = requests.get('https://virtserver.swaggerhub.com/Atomicbox/Wallet-to-ATOMIC-APIs/1.0/api/v1/users/{user-id}/unlock', params={

}, headers = headers)

print r.json()

``` -->

> Example responses

> 200 Response

```json
{
  "raw-transaction": "02000000018e62016f5fe7fad1f7f343cdf900f8c6aad4cd1bef9c2a274334291c3999551c0000000000feffffff0118c69a3b0000000017a91455f415046dfb9e578426fbcbfb480a66b0782480876e000000"
}
```

### Return Codes
Status|Meaning|Description
---|---|---
200 | OK | On demand unlock transaction created
403 | Forbidden | Cannot perform on-demand unlock at this time
404 | Not Found | User not found

## Submit an on-demand unlock

Perform an unlock of funds from the user's private-collateral address.

Should supply a signed unlock transaction which was obtained from the "GET" command, or created manually

`PUT /api/v1/users/{user-id}/unlock`

<aside class="warning">
NOTE: An on demand unlock cannot be processed during ongoing transactions
</aside>


> Body parameters

```json
{
    "partially-signed-unlock-tx": "02000000018e62016f5fe7fad1f7f343cdf900f8c6aad4cd1bef9c2a274334291c3999551c00000000b400483045022100de328bf0550164f0b6f1ae3bae78c6d417140808fa82f0da67be00cd8df41816022024b2fc2182789488fcca4984e7af3123bd3e680e4f4be8eaeb3e547f24ca31ea01004c675241043af1ba0a3429d7890d91a7f9ff280b7130c3b5edd1c02613e9f770a05ec1977a76646d7223ff17f3881f07e08a15f31cb23e79a7871202e52c678f5b9af6823021033f1654d3187087ad5f7f220814c94232945825663d55431345aca8a82746780552aefeffffff0118c69a3b0000000017a91455f415046dfb9e578426fbcbfb480a66b0782480876e000000"
}
```

> Code samples
```shell
curl -request PUT https://atomic.org/wallet-to-atomic/api/v1/users/{user-id}/unlock 
--data '{"partially-signed-unlock-tx":      "02000000018e62016f5fe7fad1f7f343cdf900f8c6aad4cd1bef9c2a274334291c3999551c00000000b400483045022100de328bf0550164f0b6f1ae3bae78c6d417140808fa82f0da67be00cd8df41816022024b2fc2182789488fcca4984e7af3123bd3e680e4f4be8eaeb3e547f24ca31ea01004c675241043af1ba0a3429d7890d91a7f9ff280b7130c3b5edd1c02613e9f770a05ec1977a76646d7223ff17f3881f07e08a15f31cb23e79a7871202e52c678f5b9af6823021033f1654d3187087ad5f7f220814c94232945825663d55431345aca8a82746780552aefeffffff0118c69a3b0000000017a91455f415046dfb9e578426fbcbfb480a66b0782480876e000000"}'
```

<!--
```python
import requests
headers = {
  'Content-Type': 'application/json'
}

r = requests.put('https://virtserver.swaggerhub.com/Atomicbox/Wallet-to-ATOMIC-APIs/1.0/api/v1/users/{user-id}/unlock', params={

}, headers = headers)

print r.json()

``` -->

> Example responses

> 201 Response -  Unlock command recieved

### Return Codes
Status|Meaning|Description|
--------- |  ----------- | -----------
201| Created|Unlock command recieved
400| Bad Request | Invalid input, object invalid
403| Forbidden|Cannot perform unlock at this time
404 |Not Found | User not found
409| Conflict | Another unlock already in progress


## Get timed unlock

Get a timed unlock transaction from the user's Private Coilateral address at the given lock time

`GET /api/v1/users/{user-id}/unlock/timed`

<aside class="warning">
Transactions will not be allowed near the target unlock time
</aside>

> Body parameters

```json
{
    "amount": 498000000,
    "nLockTime": 1548885799
}
```

> Code samples

```shell
curl -request GET https://atomic.org/wallet-to-atomic/api/v1/users/{user-id}/unlock/timed 
--data '{"amount": 498000000, "nLockTime": 1548885799}'  
```
<!--
```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json'
}

r = requests.get('https://virtserver.swaggerhub.com/Atomicbox/Wallet-to-ATOMIC-APIs/1.0/api/v1/users/{user-id}/unlock/timed', params={

}, headers = headers)

print r.json()

``` -->

> Example responses

> 200 Response

```json
{
  "raw-transaction": "02000000018e62016f5fe7fad1f7f343cdf900f8c6aad4cd1bef9c2a274334291c3999551c0000000000feffffff0118c69a3b0000000017a91455f415046dfb9e578426fbcbfb480a66b0782480876e000000"
}
```
### Return Codes
Status|Meaning|Description
---|---|---
200 | OK | Transaction created
400 | Bad Request | Invalid input, object invalid
403 | Forbidden | Cannot create the requested timed unlock
404|  Not Found | User not found 


## Submit timed unlock

Requests ATOMIC to sign the user-signed timed unlock transaction.

<aside class="success">
The timed transaction will be stored by the user, and submitted when the lock time arrives
</aside>


`PUT /api/v1/users/{user-id}/unlock/timed`


> Body parameters

```json
{
    "partially-signed-tx": "02000000018e62016f5fe7fad1f7f343cdf900f8c6aad4cd1bef9c2a274334291c3999551c00000000b400483045022100de328bf0550164f0b6f1ae3bae78c6d417140808fa82f0da67be00cd8df41816022024b2fc2182789488fcca4984e7af3123bd3e680e4f4be8eaeb3e547f24ca31ea01004c675241043af1ba0a3429d7890d91a7f9ff280b7130c3b5edd1c02613e9f770a05ec1977a76646d7223ff17f3881f07e08a15f31cb23e79a7871202e52c678f5b9af6823021033f1654d3187087ad5f7f220814c94232945825663d55431345aca8a82746780552aefeffffff0118c69a3b0000000017a91455f415046dfb9e578426fbcbfb480a66b0782480876e000000"
}
```

> Code samples

```shell
curl -request PUT https://atomic.org/wallet-to-atomic/api/v1/users/{user-id}/unlock/timed 
--data '{"partially-signed-tx": "02000000018e62016f5fe7fad1f7f343cdf900f8c6aad4cd1bef9c2a274334291c3999551c00000000b400483045022100de328bf0550164f0b6f1ae3bae78c6d417140808fa82f0da67be00cd8df41816022024b2fc2182789488fcca4984e7af3123bd3e680e4f4be8eaeb3e547f24ca31ea01004c675241043af1ba0a3429d7890d91a7f9ff280b7130c3b5edd1c02613e9f770a05ec1977a76646d7223ff17f3881f07e08a15f31cb23e79a7871202e52c678f5b9af6823021033f1654d3187087ad5f7f220814c94232945825663d55431345aca8a82746780552aefeffffff0118c69a3b0000000017a91455f415046dfb9e578426fbcbfb480a66b0782480876e000000"}'  
```
<!--
```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': '*/*'
}

r = requests.put('https://virtserver.swaggerhub.com/Atomicbox/Wallet-to-ATOMIC-APIs/1.0/api/v1/users/{user-id}/unlock/timed', params={

}, headers = headers)

print r.json()

``` -->

### Return Codes
Status|Meaning|Description
---|---|---
201| Created | Unlock command received
400| Bad Request | Invalid input, object invalid
403| Forbidden | Cannot perform unlock, earlier unlock/transaction is already in progress
404| Not Found | User not found
409| Conflict | Already exists 


# Payments


## Get payment details

Gets the required information for payment confirmation.

`GET /api/v1/users/{user-id}/payments/{payment-id}`

<aside class="success">
ATOMIC will provide the raw escrow transaction, as well as the required details for the payment transaction
</aside>


> Code samples

```shell
curl -request GET https://atomic.org/wallet-to-atomic/api/v1/users/{user-id}/payments/{payment-id} 
```
<!--
```python
import requests
headers = {
  'Accept': 'application/json'
}

r = requests.get('https://virtserver.swaggerhub.com/Atomicbox/Wallet-to-ATOMIC-APIs/1.0/api/v1/users/{user-id}/payments/{payment-id}', params={
}, headers = headers)

print r.json()

``` -->

> Example responses

> 200 Response

```json
{
  "payment-transaction-details": {
    "coin-type": "BTC",
    "fee": 1000,
    "outputs": [
      {
        "address": "2N82m2L7p17jNzrnJKN4CTjuAApSYmRtPe2",
        "amount": 1000000
      }
    ]
  },
  "escrow-transaction-details": {
    "coin-type": "BTC",
    "source-address": "2Mx1EM2Bsd8qzV2yFv3XxemKaqW7ZfCKAFN",
    "raw-escrow-transaction": "0200000001fea70fbb492e7814ce3a31639c64a7819a95ee43580867cc751a3e83438289fb0000000000ffffffff0200c2eb0b0000000017a9144f22a11a711f126454fd24e9d589529ed592c93387dc13ae2f0000000017a914119cd81d4e3c2fab3e7edb5556ddaf5d1df893248700000000"
  }
}
```

### Return Codes
Status|Meaning|Description
---|---|---
200| OK | Payment found
400| Bad Request | Invalid input, object invalid
404| Not Found | User/Payment not found
409| Conflict | Payment already performed


## Confirm payment

Confirms the payment.

Providing ATOMIC with the signed escrow transactions which was obtained from the "GET" command, and the created and signed pay transaction

`PUT /api/v1/users/{user-id}/payments/{payment-id}`


> Body parameters

```json
{
    "fully-signed-pay-transaction": "02000000000101fea70fbb492e7814ce3a31639c64a7819a95ee43580867cc751a3e83438289fb01000000232200200818c20455783996fd6c4d753751f4c3f49d9a474fed9a65c3ddb31a48afa7f8ffffffff0200e1f5050000000017a9144f22a11a711f126454fd24e9d589529ed592c93387b800a3350000000017a914e90e71bb600bb67ac5c67b0f512c521dc1d7eceb87030047304402206f67c13ae9c8ad897090f8660993adabec91c1a221558f7a0fd46fe5054d911d02205c28139ab369be614588923742ea9b3922701240c1e19ab27983a1755ca8653c01255121032cea817f794f4afa7061a1187bb8108bfff0096c61f69468ce800629cebec87951ae00000000",
    
    "partially-signed-escrow-transaction": "0200000001fea70fbb492e7814ce3a31639c64a7819a95ee43580867cc751a3e83438289fb00000000b400483045022100c37d9f6bef9575be912e1963815f50efaa269779d7edcc72333044f14b3c52500220785d9bb8636125e30ca20a3b300b02f9566b33e0d5610b49393187a9c9ea603d01004c675241041256ae5095da25836abbe95f06f54ec27a745d66df3baa98f7ca1c54bfd12e26b279680220f779d059c3f168a7bbbc35344f5ff82b58d8329e920daba2586b3121032cea817f794f4afa7061a1187bb8108bfff0096c61f69468ce800629cebec87952aeffffffff0200c2eb0b0000000017a9144f22a11a711f126454fd24e9d589529ed592c93387dc13ae2f0000000017a914119cd81d4e3c2fab3e7edb5556ddaf5d1df893248700000000"
}
```

> Code samples

```shell
curl -request PUT https://atomic.org/wallet-to-atomic/api/v1/users/{user-id}/payments/{payment-id} 
--data '{"fully-signed-pay-transaction": "02000000000101fea70fbb492e7814ce3a31639c64a7819a95ee43580867cc751a3e83438289fb01000000232200200818c20455783996fd6c4d753751f4c3f49d9a474fed9a65c3ddb31a48afa7f8ffffffff0200e1f5050000000017a9144f22a11a711f126454fd24e9d589529ed592c93387b800a3350000000017a914e90e71bb600bb67ac5c67b0f512c521dc1d7eceb87030047304402206f67c13ae9c8ad897090f8660993adabec91c1a221558f7a0fd46fe5054d911d02205c28139ab369be614588923742ea9b3922701240c1e19ab27983a1755ca8653c01255121032cea817f794f4afa7061a1187bb8108bfff0096c61f69468ce800629cebec87951ae00000000",

"partially-signed-escrow-transaction": "0200000001fea70fbb492e7814ce3a31639c64a7819a95ee43580867cc751a3e83438289fb00000000b400483045022100c37d9f6bef9575be912e1963815f50efaa269779d7edcc72333044f14b3c52500220785d9bb8636125e30ca20a3b300b02f9566b33e0d5610b49393187a9c9ea603d01004c675241041256ae5095da25836abbe95f06f54ec27a745d66df3baa98f7ca1c54bfd12e26b279680220f779d059c3f168a7bbbc35344f5ff82b58d8329e920daba2586b3121032cea817f794f4afa7061a1187bb8108bfff0096c61f69468ce800629cebec87952aeffffffff0200c2eb0b0000000017a9144f22a11a711f126454fd24e9d589529ed592c93387dc13ae2f0000000017a914119cd81d4e3c2fab3e7edb5556ddaf5d1df893248700000000"
}'
```

<!--
```python
import requests
headers = {
  'Content-Type': 'application/json'
}

r = requests.put('https://virtserver.swaggerhub.com/Atomicbox/Wallet-to-ATOMIC-APIs/1.0/api/v1/users/{user-id}/payments/{payment-id}', params={

}, headers = headers)

print r.json()

``` -->

### Return Codes
Status|Meaning|Description
---|---|---
201| Created | Payment received 
400| Bad Request  | Invalid input, object invalid
404| Not Found  | User/Payment not found
409| Conflict  | Payment already performed 

# General Information

## Get ATOMIC generic information

Get general ATOMIC information

`GET /api/v1/info`

> Code samples

```shell
curl -request GET https://atomic.org/wallet-to-atomic/api/v1/info 
```
<!--
```python
import requests
headers = {
  'Accept': 'application/json'
}

r = requests.get('https://virtserver.swaggerhub.com/Atomicbox/Wallet-to-ATOMIC-APIs/1.0/api/v1/info', params={

}, headers = headers)

print r.json()

``` -->

> Example responses

> 200 Response

```json
{
  "api-version": "v1"
}
```

### Return Codes
Status|Meaning|Description
---|---|---
200 |OK | OK
400|Bad Request | Bad input parameter


# Trade Platform APIs
 
 This section will detail the APIs from an ATOMIC powered wallet to an ATOMIC powered Trading Platform
 
## Create Order
 
 Place a new trade order

 `PUT /api/trades/v1/users/{user-id}/orders/{order-id}`

> Body parameters

```json
{
    "source-coin": "BTC",
    "source-amount": 1010000000,
    "destination-coin": "ETH",
    "destination-amount": 300500000000000000000,
    "destination-address": "0xe8e49e84480edd95aaae50a340422cb30057963b",
    "escrow-coin": "BTC",
    "escrow-amount": 1010000000,
    "escrow-address": "2NF1qc9L4j1tyfqzzLdRzRqoVftHznSeC6K"
}
```

 > Code samples
 
 ```shell
 curl -request PUT https://atomic.org/api/trades/v1/users/{user-id}/orders/{order-id} 
 --data '{
     "source-coin": "BTC",
     "source-amount": 1010000000,
     "destination-coin": "ETH",
     "destination-amount": 300500000000000000000,
     "destination-address": "0xe8e49e84480edd95aaae50a340422cb30057963b",
     "escrow-coin": "BTC",
     "escrow-amount": 1010000000,
     "escrow-address": "2NF1qc9L4j1tyfqzzLdRzRqoVftHznSeC6K"
     }'
 ```
### Return Codes
 Status|Meaning|Description
 ---|---|---
 201 | Created | Order created succesfully 
 400 | Bad Request | Invalid input
 409 | Conflict | Order already exists


## Get Order Details

Retrieves the existing information for a specific order.

If the order is currently in an active deal - will also return the payment ID

`GET /api/trades/v1/users/{user-id}/orders/{order-id}`

> Code samples

```shell
curl -request GET https://atomic.org/api/trades/v1/users/{user-id}/orders/{order-id} 
```

> Example responses

> 200 Response

```json
{
    "order-id": "d290f1ee-6c54-4b01-90e6-d701748f0100",
    "user-id": "d290f1ee-6c54-4b01-90e6-d701748f0200",
    "status": "Pending match",
    "payment-id": "d290f1ee-6c54-4b01-90e6-d701748f0300"
}
```
### Return Codes
Status|Meaning|Description
---|---|---
200 | OK | Order found
404 | Not Found | Order not found

## Cancel Order

Deletes a given order, if allowed

`DELETE /api/trades/v1/users/{user-id}/orders/{order-id}`

> Code samples

```shell
curl -request DELETE https://atomic.org/api/trades/v1/users/{user-id}/orders/{order-id}

```
### Return Codes
Status|Meaning|Description
---|---|---
200 | OK | Order canceled
400 | Bad Request | Invalid input
403 | Forbidden | Order is currently being executed, unable to cancel 
404 | Not Found | Order not found

## Get Supported Coins

List of supported coins - by default all pairs are supported

`GET /api/trades/v1/coins`

> Code samples

```shell
curl -request GET https://atomic.org/api/trades/v1/coins'
```

> Example responses

> 200 Response
```json
{
   [BTC, LTC, DASH, BSV, DOGE, BCH, ETC, ETH] 
}
```

## Get Generic Info

Generic info from the trading platform

`GET /api/trades/v1/info`

> Code samples

```shell
curl -request https://atomic.org/api/trades/v1/info'
```


> Example responses

> 200 Response

```json
{
    "business-name": "Bestrade - P2P trading",
    "business-id": "d290f1ee-6c54-4b01-90e6-d701748f0400",
    "api-version": "v1"
}
```

# Credit Line Platforms APIs

## Request Credit Line


Request/Update Credit Line amount.

<aside class="success">
Returns a payment ID in case such is required
</aside>

`PUT /api/credit-line/v1/users/{user-id}/credits/{credit-id}`


> Body parameters

```json
{
    "coin-type": "BTC",
    "amount": 1050000000
}
```

> Code samples

```shell
curl -request PUT  PUT https://atomic.org/api/credit-line/v1/users/{user-id}/credits/{credit-id} \
--data '{ "coin-type": "BTC", "amount": 1050000000}'
```

> Example responses

> 201 Response

```json
{
    "user-id": "d290f1ee-6c54-4b01-90e6-d701748f0200",
    "amount": 1050000000,
    "coin-type": "BTC",
    "payment-id": "d290f1ee-6c54-4b01-90e6-d701748f0300"
}
```
### Return Codes
Status|Meaning|Description
---|---|---
201 | Created | Credit Line updated - escrow required
400 | Bad Request | Invalid input
403 | Forbidden  | Can't update Credit Line to be lower than existing
409 | Conflict | Credit Line up to date - no additional escrow required


## Get Credit Line Details

Gets the user's credit-line information - with the current payment ID if requried

`GET /api/credit-line/v1/users/{user-id}/credits/{credit-id}`

> Code samples

```shell
curl -request GET  PUT https://atomic.org/api/credit-line/v1/users/{user-id}/credits/{credit-id}
```

> Example responses

> 200 Response

```json
{
    "user-id": "d290f1ee-6c54-4b01-90e6-d701748f0200",
    "amount": 1050000000,
    "coin-type": "BTC",
    "payment-id": "d290f1ee-6c54-4b01-90e6-d701748f0300"
}
```

### Return Codes
Status|Meaning|Description
---|---|---
200| OK | Credit Line found
404| Not Found | User/Credit Line not found


## Delete Credit Line

Request to remove a specific credit-line

`DELETE /api/credit-line/v1/users/{user-id}/credits/{credit-id}`

> Code samples

```shell
curl -request DELETE  PUT https://atomic.org/api/credit-line/v1/users/{user-id}/credits/{credit-id}

```
### Return Codes
Status|Meaning|Description|Schema|
---|---|---|---|
200| OK | Credit Line deleted succesfully
403| Forbidden | Credit Line not yet settled - delete not allowed
404| Not Found | User/Credit Line not found

