---
title: Wallet to ATOMIC APIs
language_tabs:
  - python: Python
toc_footers:
  []
includes:
  []
search: true
highlight_theme: darkula
headingLevel: 2

---

<!-- Generator: Widdershins v3.6.6 -->

<section>
<h1 id="wallet-to-atomic-apis">Wallet to ATOMIC APIs v1.0</h1>

> Scroll down for code samples, example requests and responses. Select a language for code samples from the tabs above or the mobile navigation menu.

APIs from an ATOMIC powered wallet towards ATOMIC P2P collateral guarantees service

Base URLs:

* <a href="https://virtserver.swaggerhub.com/Atomicbox/Wallet-to-ATOMIC-APIs/1.0">https://virtserver.swaggerhub.com/Atomicbox/Wallet-to-ATOMIC-APIs/1.0</a>

</section>

<section>
<h1 id="wallet-to-atomic-apis-user-management">user-management</h1>

User management related APIs

<section>

## Get user's information

<a id="opIdgetUserInfo"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

r = requests.get('https://virtserver.swaggerhub.com/Atomicbox/Wallet-to-ATOMIC-APIs/1.0/api/v1/users/{user-id}', params={

}, headers = headers)

print r.json()

```

`GET /api/v1/users/{user-id}`

Retrieves existing information for a specific user

<section>
<h3 id="get-user's-information-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|user-id|path|string(uuid)|true|User ID|

</section>

> Example responses

> 200 Response

```json
{
  "smart-credit-info": {
    "atomic-public-key": "022d4a565aa6845c72bd2a732d3d4971704e8403942766b59d68e0dcf62713443c",
    "smart-credit-address": "2Mx1EM2Bsd8qzV2yFv3XxemKaqW7ZfCKAFN",
    "smart-credit-redeem-script": "5221022d4a565aa6845c72bd2a732d3d4971704e8403942766b59d68e0dcf62713443c21027a478fd4a7c3b8e43cfd2eaf0e439158b2b874dace2b598c585e88f67eeb865552ae",
    "smart-credit-script-pub-key": "a91434331a9a1282cb1f58a383eb3d74bbbe671161ba87"
  },
  "smart-credit-balance": 102340000,
  "inflight-transactions": [
    {
      "payment-id": "d290f1ee-6c54-4b01-90e6-d701748f0300",
      "transaction-id": "850b5f43b0b13837f2fea92818c73034c7337c963245eaa4eca8b78e00515fd0",
      "status": "PENDING-CONFIRMATIONS"
    }
  ]
}
```

<section>
<h3 id="get-user's-information-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|user found|[user-get-response](#schemauser-get-response)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|user not found|None|

</section>

<aside class="success">
This operation does not require authentication
</aside>

</section>

<section>

## Create new user

<a id="opIdregisterUser"></a>

> Code samples

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json'
}

r = requests.put('https://virtserver.swaggerhub.com/Atomicbox/Wallet-to-ATOMIC-APIs/1.0/api/v1/users/{user-id}', params={

}, headers = headers)

print r.json()

```

`PUT /api/v1/users/{user-id}`

Register user with ATOMIC - creating a smart-credit address

> Body parameter

```json
{
  "public-key": "027a478fd4a7c3b8e43cfd2eaf0e439158b2b874dace2b598c585e88f67eeb8655",
  "refund-address": "2NF1qc9L4j1tyfqzzLdRzRqoVftHznSeC6K"
}
```

<section>
<h3 id="create-new-user-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|user-id|path|string(uuid)|true|User ID|
|body|body|[register-user-params](#schemaregister-user-params)|false|Parameters for user registeration|

</section>

> Example responses

> 201 Response

```json
{
  "smart-credit-info": {
    "atomic-public-key": "022d4a565aa6845c72bd2a732d3d4971704e8403942766b59d68e0dcf62713443c",
    "smart-credit-address": "2Mx1EM2Bsd8qzV2yFv3XxemKaqW7ZfCKAFN",
    "smart-credit-redeem-script": "5221022d4a565aa6845c72bd2a732d3d4971704e8403942766b59d68e0dcf62713443c21027a478fd4a7c3b8e43cfd2eaf0e439158b2b874dace2b598c585e88f67eeb865552ae",
    "smart-credit-script-pub-key": "a91434331a9a1282cb1f58a383eb3d74bbbe671161ba87"
  }
}
```

<section>
<h3 id="create-new-user-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|user registered|[register-user-response](#schemaregister-user-response)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|invalid input, object invalid|None|
|409|[Conflict](https://tools.ietf.org/html/rfc7231#section-6.5.8)|user already exists|None|

</section>

<aside class="success">
This operation does not require authentication
</aside>

</section>

<section>

## Get on-demand unlock

<a id="opIdgetUnlock"></a>

> Code samples

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json'
}

r = requests.get('https://virtserver.swaggerhub.com/Atomicbox/Wallet-to-ATOMIC-APIs/1.0/api/v1/users/{user-id}/unlock', params={

}, headers = headers)

print r.json()

```

`GET /api/v1/users/{user-id}/unlock`

Get an on-demand unlock transaction from the user's smart credit address

> Body parameter

```json
{
  "amount": 498000000
}
```

<section>
<h3 id="get-on-demand-unlock-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|user-id|path|string(uuid)|true|User ID|
|body|body|[get-unlock-params](#schemaget-unlock-params)|false|Parameters for getting an unlock transaction|

</section>

> Example responses

> 200 Response

```json
{
  "raw-transaction": "02000000018e62016f5fe7fad1f7f343cdf900f8c6aad4cd1bef9c2a274334291c3999551c0000000000feffffff0118c69a3b0000000017a91455f415046dfb9e578426fbcbfb480a66b0782480876e000000"
}
```

<section>
<h3 id="get-on-demand-unlock-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|transaction created|[get-unlock-response](#schemaget-unlock-response)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|invalid input, object invalid|None|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|cannot perform unlock at this time|None|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|user not found|None|

</section>

<aside class="success">
This operation does not require authentication
</aside>

</section>

<section>

## Submit an on-demand unlock

<a id="opIdperformUnlock"></a>

> Code samples

```python
import requests
headers = {
  'Content-Type': 'application/json'
}

r = requests.put('https://virtserver.swaggerhub.com/Atomicbox/Wallet-to-ATOMIC-APIs/1.0/api/v1/users/{user-id}/unlock', params={

}, headers = headers)

print r.json()

```

`PUT /api/v1/users/{user-id}/unlock`

Perform an unlock of funds from the user's smart-credit.
Should supply a signed unlock transaction which was obtained from the "GET" command, or created manually

> Body parameter

```json
{
  "partially-signed-unlock-tx": "02000000018e62016f5fe7fad1f7f343cdf900f8c6aad4cd1bef9c2a274334291c3999551c00000000b400483045022100de328bf0550164f0b6f1ae3bae78c6d417140808fa82f0da67be00cd8df41816022024b2fc2182789488fcca4984e7af3123bd3e680e4f4be8eaeb3e547f24ca31ea01004c675241043af1ba0a3429d7890d91a7f9ff280b7130c3b5edd1c02613e9f770a05ec1977a76646d7223ff17f3881f07e08a15f31cb23e79a7871202e52c678f5b9af6823021033f1654d3187087ad5f7f220814c94232945825663d55431345aca8a82746780552aefeffffff0118c69a3b0000000017a91455f415046dfb9e578426fbcbfb480a66b0782480876e000000"
}
```

<section>
<h3 id="submit-an-on-demand-unlock-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|user-id|path|string(uuid)|true|User ID|
|body|body|[put-unlock-params](#schemaput-unlock-params)|false|Parameters for performing the unlock command|

</section>

<section>
<h3 id="submit-an-on-demand-unlock-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|unlock command recieved|None|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|invalid input, object invalid|None|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|cannot perform unlock, another unlock/transaction is already in progress|None|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|user not found|None|
|409|[Conflict](https://tools.ietf.org/html/rfc7231#section-6.5.8)|already exists|None|

</section>

<aside class="success">
This operation does not require authentication
</aside>

</section>

<section>

## Get timed unlock

<a id="opIdgetTimedUnlock"></a>

> Code samples

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json'
}

r = requests.get('https://virtserver.swaggerhub.com/Atomicbox/Wallet-to-ATOMIC-APIs/1.0/api/v1/users/{user-id}/unlock/timed', params={

}, headers = headers)

print r.json()

```

`GET /api/v1/users/{user-id}/unlock/timed`

Get a timed unlock transaction from the user's smart credit address at the given lock time

> Body parameter

```json
{
  "amount": 498000000,
  "nLockTime": 1548885799
}
```

<section>
<h3 id="get-timed-unlock-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|user-id|path|string(uuid)|true|User ID|
|body|body|[get-timed-unlock-params](#schemaget-timed-unlock-params)|false|Parameters for getting a timed unlock transaction|

</section>

> Example responses

> 200 Response

```json
{
  "raw-transaction": "02000000018e62016f5fe7fad1f7f343cdf900f8c6aad4cd1bef9c2a274334291c3999551c0000000000feffffff0118c69a3b0000000017a91455f415046dfb9e578426fbcbfb480a66b0782480876e000000"
}
```

<section>
<h3 id="get-timed-unlock-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|transaction created|[get-timed-unlock-response](#schemaget-timed-unlock-response)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|invalid input, object invalid|None|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|cannot perform the requested timed unlock|None|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|user not found|None|

</section>

<aside class="success">
This operation does not require authentication
</aside>

</section>

<section>

## Submit timed unlock

<a id="opIdperformTimedUnlock"></a>

> Code samples

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': '*/*'
}

r = requests.put('https://virtserver.swaggerhub.com/Atomicbox/Wallet-to-ATOMIC-APIs/1.0/api/v1/users/{user-id}/unlock/timed', params={

}, headers = headers)

print r.json()

```

`PUT /api/v1/users/{user-id}/unlock/timed`

Requests ATOMIC to sign the user-signed timed unlock transaction.
The timed transaction will be stored by the user, and submitted when the lock time arrives

> Body parameter

```json
{
  "partially-signed-tx": "02000000018e62016f5fe7fad1f7f343cdf900f8c6aad4cd1bef9c2a274334291c3999551c00000000b400483045022100de328bf0550164f0b6f1ae3bae78c6d417140808fa82f0da67be00cd8df41816022024b2fc2182789488fcca4984e7af3123bd3e680e4f4be8eaeb3e547f24ca31ea01004c675241043af1ba0a3429d7890d91a7f9ff280b7130c3b5edd1c02613e9f770a05ec1977a76646d7223ff17f3881f07e08a15f31cb23e79a7871202e52c678f5b9af6823021033f1654d3187087ad5f7f220814c94232945825663d55431345aca8a82746780552aefeffffff0118c69a3b0000000017a91455f415046dfb9e578426fbcbfb480a66b0782480876e000000"
}
```

<section>
<h3 id="submit-timed-unlock-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|user-id|path|string(uuid)|true|User ID|
|body|body|[put-timed-unlock-params](#schemaput-timed-unlock-params)|false|Parameters for performing the unlock command|

</section>

> Example responses

> 201 Response

<section>
<h3 id="submit-timed-unlock-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|unlock command received|[put-timed-unlock-response](#schemaput-timed-unlock-response)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|invalid input, object invalid|None|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|cannot perform unlock, earlier unlock/transaction is already in progress|None|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|user not found|None|
|409|[Conflict](https://tools.ietf.org/html/rfc7231#section-6.5.8)|already exists|[put-timed-unlock-response](#schemaput-timed-unlock-response)|

</section>

<aside class="success">
This operation does not require authentication
</aside>

</section>

</section>

<section>
<h1 id="wallet-to-atomic-apis-transactions">transactions</h1>

APIs for transactions of ATOMIC payments

<section>

## Get payment details

<a id="opIdgetPaymentInfo"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

r = requests.get('https://virtserver.swaggerhub.com/Atomicbox/Wallet-to-ATOMIC-APIs/1.0/api/v1/users/{user-id}/payments/{payment-id}', params={

}, headers = headers)

print r.json()

```

`GET /api/v1/users/{user-id}/payments/{payment-id}`

Gets the information needed to confirm an order.
ATOMIC will provide the raw escrow transaction, as well as the required details for the payment

<section>
<h3 id="get-payment-details-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|user-id|path|string(uuid)|true|User ID|
|payment-id|path|string(uuid)|true|Payment ID|

</section>

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

<section>
<h3 id="get-payment-details-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|payment found|[get-payment-response](#schemaget-payment-response)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|invalid input, object invalid|None|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|user/payment not found|None|
|409|[Conflict](https://tools.ietf.org/html/rfc7231#section-6.5.8)|payment already performed|None|

</section>

<aside class="success">
This operation does not require authentication
</aside>

</section>

<section>

## Confirm payment

<a id="opIdconfirmPayment"></a>

> Code samples

```python
import requests
headers = {
  'Content-Type': 'application/json'
}

r = requests.put('https://virtserver.swaggerhub.com/Atomicbox/Wallet-to-ATOMIC-APIs/1.0/api/v1/users/{user-id}/payments/{payment-id}', params={

}, headers = headers)

print r.json()

```

`PUT /api/v1/users/{user-id}/payments/{payment-id}`

Confirms the payment.
Providing ATOMIC with the signed transactions which were obtained from the "GET" command

> Body parameter

```json
{
  "fully-signed-pay-transaction": "02000000000101fea70fbb492e7814ce3a31639c64a7819a95ee43580867cc751a3e83438289fb01000000232200200818c20455783996fd6c4d753751f4c3f49d9a474fed9a65c3ddb31a48afa7f8ffffffff0200e1f5050000000017a9144f22a11a711f126454fd24e9d589529ed592c93387b800a3350000000017a914e90e71bb600bb67ac5c67b0f512c521dc1d7eceb87030047304402206f67c13ae9c8ad897090f8660993adabec91c1a221558f7a0fd46fe5054d911d02205c28139ab369be614588923742ea9b3922701240c1e19ab27983a1755ca8653c01255121032cea817f794f4afa7061a1187bb8108bfff0096c61f69468ce800629cebec87951ae00000000",
  "partially-signed-escrow-transaction": "0200000001fea70fbb492e7814ce3a31639c64a7819a95ee43580867cc751a3e83438289fb00000000b400483045022100c37d9f6bef9575be912e1963815f50efaa269779d7edcc72333044f14b3c52500220785d9bb8636125e30ca20a3b300b02f9566b33e0d5610b49393187a9c9ea603d01004c675241041256ae5095da25836abbe95f06f54ec27a745d66df3baa98f7ca1c54bfd12e26b279680220f779d059c3f168a7bbbc35344f5ff82b58d8329e920daba2586b3121032cea817f794f4afa7061a1187bb8108bfff0096c61f69468ce800629cebec87952aeffffffff0200c2eb0b0000000017a9144f22a11a711f126454fd24e9d589529ed592c93387dc13ae2f0000000017a914119cd81d4e3c2fab3e7edb5556ddaf5d1df893248700000000"
}
```

<section>
<h3 id="confirm-payment-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|user-id|path|string(uuid)|true|User ID|
|payment-id|path|string(uuid)|true|Payment ID|
|body|body|[put-payment-params](#schemaput-payment-params)|false|Parameters for performing the payment|

</section>

<section>
<h3 id="confirm-payment-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|payment received|None|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|invalid input, object invalid|None|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|user/payment not found|None|
|409|[Conflict](https://tools.ietf.org/html/rfc7231#section-6.5.8)|payment already performed|None|

</section>

<aside class="success">
This operation does not require authentication
</aside>

</section>

</section>

<section>
<h1 id="wallet-to-atomic-apis-info">info</h1>

Generic ATOMIC info

<section>

## Get ATOMIC generic information

<a id="opIdgetAtomicInfo"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

r = requests.get('https://virtserver.swaggerhub.com/Atomicbox/Wallet-to-ATOMIC-APIs/1.0/api/v1/info', params={

}, headers = headers)

print r.json()

```

`GET /api/v1/info`

Get general ATOMIC information

> Example responses

> 200 Response

```json
{
  "api-version": "v1"
}
```

<section>
<h3 id="get-atomic-generic-information-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[atomic-generic-info](#schemaatomic-generic-info)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|bad input parameter|None|

</section>

<aside class="success">
This operation does not require authentication
</aside>

</section>

</section>

<section>

# Schemas

<section>
<h2 id="tocS_register-user-params">register-user-params</h2>
<!-- backwards compatibility -->
<a id="schemaregister-user-params"></a>
<a id="schema_register-user-params"></a>
<a id="tocSregister-user-params"></a>
<a id="tocsregister-user-params"></a>

```json
{
  "public-key": "027a478fd4a7c3b8e43cfd2eaf0e439158b2b874dace2b598c585e88f67eeb8655",
  "refund-address": "2NF1qc9L4j1tyfqzzLdRzRqoVftHznSeC6K"
}

```

register-user-params

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|public-key|[public-key](#schemapublic-key)|true|none|Public key|
|refund-address|string|true|none|User's address to which unlock transactions will be sent|

</section>
</section>

<section>
<h2 id="tocS_register-user-response">register-user-response</h2>
<!-- backwards compatibility -->
<a id="schemaregister-user-response"></a>
<a id="schema_register-user-response"></a>
<a id="tocSregister-user-response"></a>
<a id="tocsregister-user-response"></a>

```json
{
  "smart-credit-info": {
    "atomic-public-key": "022d4a565aa6845c72bd2a732d3d4971704e8403942766b59d68e0dcf62713443c",
    "smart-credit-address": "2Mx1EM2Bsd8qzV2yFv3XxemKaqW7ZfCKAFN",
    "smart-credit-redeem-script": "5221022d4a565aa6845c72bd2a732d3d4971704e8403942766b59d68e0dcf62713443c21027a478fd4a7c3b8e43cfd2eaf0e439158b2b874dace2b598c585e88f67eeb865552ae",
    "smart-credit-script-pub-key": "a91434331a9a1282cb1f58a383eb3d74bbbe671161ba87"
  }
}

```

register-user-response

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|smart-credit-info|[smart-credit-info](#schemasmart-credit-info)|true|none|none|

</section>
</section>

<section>
<h2 id="tocS_smart-credit-info">smart-credit-info</h2>
<!-- backwards compatibility -->
<a id="schemasmart-credit-info"></a>
<a id="schema_smart-credit-info"></a>
<a id="tocSsmart-credit-info"></a>
<a id="tocssmart-credit-info"></a>

```json
{
  "atomic-public-key": "022d4a565aa6845c72bd2a732d3d4971704e8403942766b59d68e0dcf62713443c",
  "smart-credit-address": "2Mx1EM2Bsd8qzV2yFv3XxemKaqW7ZfCKAFN",
  "smart-credit-redeem-script": "5221022d4a565aa6845c72bd2a732d3d4971704e8403942766b59d68e0dcf62713443c21027a478fd4a7c3b8e43cfd2eaf0e439158b2b874dace2b598c585e88f67eeb865552ae",
  "smart-credit-script-pub-key": "a91434331a9a1282cb1f58a383eb3d74bbbe671161ba87"
}

```

smart-credit-info

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|atomic-public-key|string|false|none|ATOMIC's public key for the smart-credit address|
|smart-credit-address|[smart-credit-address](#schemasmart-credit-address)|true|none|The multisig address (2 out of 2) created for the smart-credit|
|smart-credit-redeem-script|string|true|none|The multisig address' redeem script|
|smart-credit-script-pub-key|string|true|none|The multisig address' script pub key|

</section>
</section>

<section>
<h2 id="tocS_inflight-transaction-info">inflight-transaction-info</h2>
<!-- backwards compatibility -->
<a id="schemainflight-transaction-info"></a>
<a id="schema_inflight-transaction-info"></a>
<a id="tocSinflight-transaction-info"></a>
<a id="tocsinflight-transaction-info"></a>

```json
{
  "payment-id": "d290f1ee-6c54-4b01-90e6-d701748f0300",
  "transaction-id": "850b5f43b0b13837f2fea92818c73034c7337c963245eaa4eca8b78e00515fd0",
  "status": "PENDING-CONFIRMATIONS"
}

```

inflight-transaction-info

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|payment-id|[payment-id](#schemapayment-id)|true|none|Payment ID|
|transaction-id|[tx-id](#schematx-id)|true|none|Transaction ID|
|status|[transaction-status](#schematransaction-status)|true|none|Transaction status in ATOMIC|

</section>
</section>

<section>
<h2 id="tocS_user-get-response">user-get-response</h2>
<!-- backwards compatibility -->
<a id="schemauser-get-response"></a>
<a id="schema_user-get-response"></a>
<a id="tocSuser-get-response"></a>
<a id="tocsuser-get-response"></a>

```json
{
  "smart-credit-info": {
    "atomic-public-key": "022d4a565aa6845c72bd2a732d3d4971704e8403942766b59d68e0dcf62713443c",
    "smart-credit-address": "2Mx1EM2Bsd8qzV2yFv3XxemKaqW7ZfCKAFN",
    "smart-credit-redeem-script": "5221022d4a565aa6845c72bd2a732d3d4971704e8403942766b59d68e0dcf62713443c21027a478fd4a7c3b8e43cfd2eaf0e439158b2b874dace2b598c585e88f67eeb865552ae",
    "smart-credit-script-pub-key": "a91434331a9a1282cb1f58a383eb3d74bbbe671161ba87"
  },
  "smart-credit-balance": 102340000,
  "inflight-transactions": [
    {
      "payment-id": "d290f1ee-6c54-4b01-90e6-d701748f0300",
      "transaction-id": "850b5f43b0b13837f2fea92818c73034c7337c963245eaa4eca8b78e00515fd0",
      "status": "PENDING-CONFIRMATIONS"
    }
  ]
}

```

user-get-response

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|smart-credit-info|[smart-credit-info](#schemasmart-credit-info)|true|none|none|
|smart-credit-balance|[balance-with-example](#schemabalance-with-example)|true|none|none|
|inflight-transactions|[[inflight-transaction-info](#schemainflight-transaction-info)]|true|none|none|

</section>
</section>

<section>
<h2 id="tocS_put-unlock-params">put-unlock-params</h2>
<!-- backwards compatibility -->
<a id="schemaput-unlock-params"></a>
<a id="schema_put-unlock-params"></a>
<a id="tocSput-unlock-params"></a>
<a id="tocsput-unlock-params"></a>

```json
{
  "partially-signed-unlock-tx": "02000000018e62016f5fe7fad1f7f343cdf900f8c6aad4cd1bef9c2a274334291c3999551c00000000b400483045022100de328bf0550164f0b6f1ae3bae78c6d417140808fa82f0da67be00cd8df41816022024b2fc2182789488fcca4984e7af3123bd3e680e4f4be8eaeb3e547f24ca31ea01004c675241043af1ba0a3429d7890d91a7f9ff280b7130c3b5edd1c02613e9f770a05ec1977a76646d7223ff17f3881f07e08a15f31cb23e79a7871202e52c678f5b9af6823021033f1654d3187087ad5f7f220814c94232945825663d55431345aca8a82746780552aefeffffff0118c69a3b0000000017a91455f415046dfb9e578426fbcbfb480a66b0782480876e000000"
}

```

put-unlock-params

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|partially-signed-unlock-tx|[partially-signed-unlock-tx](#schemapartially-signed-unlock-tx)|false|none|A transaction from a multisig (2 out of 2) address, with only one signature|

</section>
</section>

<section>
<h2 id="tocS_get-unlock-params">get-unlock-params</h2>
<!-- backwards compatibility -->
<a id="schemaget-unlock-params"></a>
<a id="schema_get-unlock-params"></a>
<a id="tocSget-unlock-params"></a>
<a id="tocsget-unlock-params"></a>

```json
{
  "amount": 498000000
}

```

get-unlock-params

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|amount|number(double)|true|none|Amount to unlock - 0 will perform a full unlock|

</section>
</section>

<section>
<h2 id="tocS_get-unlock-response">get-unlock-response</h2>
<!-- backwards compatibility -->
<a id="schemaget-unlock-response"></a>
<a id="schema_get-unlock-response"></a>
<a id="tocSget-unlock-response"></a>
<a id="tocsget-unlock-response"></a>

```json
{
  "raw-transaction": "02000000018e62016f5fe7fad1f7f343cdf900f8c6aad4cd1bef9c2a274334291c3999551c0000000000feffffff0118c69a3b0000000017a91455f415046dfb9e578426fbcbfb480a66b0782480876e000000"
}

```

get-unlock-response

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|raw-transaction|[raw-unlock-transaction](#schemaraw-unlock-transaction)|true|none|The created, unsigned, unlock transaction|

</section>
</section>

<section>
<h2 id="tocS_get-timed-unlock-params">get-timed-unlock-params</h2>
<!-- backwards compatibility -->
<a id="schemaget-timed-unlock-params"></a>
<a id="schema_get-timed-unlock-params"></a>
<a id="tocSget-timed-unlock-params"></a>
<a id="tocsget-timed-unlock-params"></a>

```json
{
  "amount": 498000000,
  "nLockTime": 1548885799
}

```

get-timed-unlock-params

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|amount|number(double)|true|none|Amount to unlock - 0 will perform a full unlock|
|nLockTime|number(integer)|true|none|Lock time for the unlock transaction|

</section>
</section>

<section>
<h2 id="tocS_get-timed-unlock-response">get-timed-unlock-response</h2>
<!-- backwards compatibility -->
<a id="schemaget-timed-unlock-response"></a>
<a id="schema_get-timed-unlock-response"></a>
<a id="tocSget-timed-unlock-response"></a>
<a id="tocsget-timed-unlock-response"></a>

```json
{
  "raw-transaction": "02000000018e62016f5fe7fad1f7f343cdf900f8c6aad4cd1bef9c2a274334291c3999551c0000000000feffffff0118c69a3b0000000017a91455f415046dfb9e578426fbcbfb480a66b0782480876e000000"
}

```

get-unlock-response

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|raw-transaction|[raw-unlock-transaction](#schemaraw-unlock-transaction)|true|none|The created, unsigned, unlock transaction|

</section>
</section>

<section>
<h2 id="tocS_put-timed-unlock-params">put-timed-unlock-params</h2>
<!-- backwards compatibility -->
<a id="schemaput-timed-unlock-params"></a>
<a id="schema_put-timed-unlock-params"></a>
<a id="tocSput-timed-unlock-params"></a>
<a id="tocsput-timed-unlock-params"></a>

```json
{
  "partially-signed-tx": "02000000018e62016f5fe7fad1f7f343cdf900f8c6aad4cd1bef9c2a274334291c3999551c00000000b400483045022100de328bf0550164f0b6f1ae3bae78c6d417140808fa82f0da67be00cd8df41816022024b2fc2182789488fcca4984e7af3123bd3e680e4f4be8eaeb3e547f24ca31ea01004c675241043af1ba0a3429d7890d91a7f9ff280b7130c3b5edd1c02613e9f770a05ec1977a76646d7223ff17f3881f07e08a15f31cb23e79a7871202e52c678f5b9af6823021033f1654d3187087ad5f7f220814c94232945825663d55431345aca8a82746780552aefeffffff0118c69a3b0000000017a91455f415046dfb9e578426fbcbfb480a66b0782480876e000000"
}

```

put-timed-unlock-params

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|partially-signed-tx|[partially-signed-unlock-tx](#schemapartially-signed-unlock-tx)|true|none|A transaction from a multisig (2 out of 2) address, with only one signature|

</section>
</section>

<section>
<h2 id="tocS_put-timed-unlock-response">put-timed-unlock-response</h2>
<!-- backwards compatibility -->
<a id="schemaput-timed-unlock-response"></a>
<a id="schema_put-timed-unlock-response"></a>
<a id="tocSput-timed-unlock-response"></a>
<a id="tocsput-timed-unlock-response"></a>

```json
{
  "fully-signed-transaction": "02000000018e62016f5fe7fad1f7f343cdf900f8c6aad4cd1bef9c2a274334291c3999551c00000000fb00473044022026a29f795c4772b6e35fe091c5c75ff5bee88fdd047a82b5e216b54e37182fb502207232f1c1fcd048f566c5e83f5b056044932d4355832106f2b11623772e6d06a901483045022100de328bf0550164f0b6f1ae3bae78c6d417140808fa82f0da67be00cd8df41816022024b2fc2182789488fcca4984e7af3123bd3e680e4f4be8eaeb3e547f24ca31ea014c675241043af1ba0a3429d7890d91a7f9ff280b7130c3b5edd1c02613e9f770a05ec1977a76646d7223ff17f3881f07e08a15f31cb23e79a7871202e52c678f5b9af6823021033f1654d3187087ad5f7f220814c94232945825663d55431345aca8a82746780552aefeffffff0118c69a3b0000000017a91455f415046dfb9e578426fbcbfb480a66b0782480876e000000"
}

```

put-timed-unlock-response

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|fully-signed-transaction|[fully-signed-unlock-tx](#schemafully-signed-unlock-tx)|true|none|A fully signed transaction from a multisig address|

</section>
</section>

<section>
<h2 id="tocS_raw-unlock-transaction">raw-unlock-transaction</h2>
<!-- backwards compatibility -->
<a id="schemaraw-unlock-transaction"></a>
<a id="schema_raw-unlock-transaction"></a>
<a id="tocSraw-unlock-transaction"></a>
<a id="tocsraw-unlock-transaction"></a>

```json
"02000000018e62016f5fe7fad1f7f343cdf900f8c6aad4cd1bef9c2a274334291c3999551c0000000000feffffff0118c69a3b0000000017a91455f415046dfb9e578426fbcbfb480a66b0782480876e000000"

```

raw-unlock-tx

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|raw-unlock-tx|string|false|none|The created, unsigned, unlock transaction|

</section>
</section>

<section>
<h2 id="tocS_partially-signed-unlock-tx">partially-signed-unlock-tx</h2>
<!-- backwards compatibility -->
<a id="schemapartially-signed-unlock-tx"></a>
<a id="schema_partially-signed-unlock-tx"></a>
<a id="tocSpartially-signed-unlock-tx"></a>
<a id="tocspartially-signed-unlock-tx"></a>

```json
"02000000018e62016f5fe7fad1f7f343cdf900f8c6aad4cd1bef9c2a274334291c3999551c00000000b400483045022100de328bf0550164f0b6f1ae3bae78c6d417140808fa82f0da67be00cd8df41816022024b2fc2182789488fcca4984e7af3123bd3e680e4f4be8eaeb3e547f24ca31ea01004c675241043af1ba0a3429d7890d91a7f9ff280b7130c3b5edd1c02613e9f770a05ec1977a76646d7223ff17f3881f07e08a15f31cb23e79a7871202e52c678f5b9af6823021033f1654d3187087ad5f7f220814c94232945825663d55431345aca8a82746780552aefeffffff0118c69a3b0000000017a91455f415046dfb9e578426fbcbfb480a66b0782480876e000000"

```

partially-signed-unlock-tx

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|partially-signed-unlock-tx|string|false|none|A transaction from a multisig (2 out of 2) address, with only one signature|

</section>
</section>

<section>
<h2 id="tocS_fully-signed-unlock-tx">fully-signed-unlock-tx</h2>
<!-- backwards compatibility -->
<a id="schemafully-signed-unlock-tx"></a>
<a id="schema_fully-signed-unlock-tx"></a>
<a id="tocSfully-signed-unlock-tx"></a>
<a id="tocsfully-signed-unlock-tx"></a>

```json
"02000000018e62016f5fe7fad1f7f343cdf900f8c6aad4cd1bef9c2a274334291c3999551c00000000fb00473044022026a29f795c4772b6e35fe091c5c75ff5bee88fdd047a82b5e216b54e37182fb502207232f1c1fcd048f566c5e83f5b056044932d4355832106f2b11623772e6d06a901483045022100de328bf0550164f0b6f1ae3bae78c6d417140808fa82f0da67be00cd8df41816022024b2fc2182789488fcca4984e7af3123bd3e680e4f4be8eaeb3e547f24ca31ea014c675241043af1ba0a3429d7890d91a7f9ff280b7130c3b5edd1c02613e9f770a05ec1977a76646d7223ff17f3881f07e08a15f31cb23e79a7871202e52c678f5b9af6823021033f1654d3187087ad5f7f220814c94232945825663d55431345aca8a82746780552aefeffffff0118c69a3b0000000017a91455f415046dfb9e578426fbcbfb480a66b0782480876e000000"

```

fully-signed-unlock-tx

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|fully-signed-unlock-tx|string|false|none|A fully signed transaction from a multisig address|

</section>
</section>

<section>
<h2 id="tocS_get-payment-params">get-payment-params</h2>
<!-- backwards compatibility -->
<a id="schemaget-payment-params"></a>
<a id="schema_get-payment-params"></a>
<a id="tocSget-payment-params"></a>
<a id="tocsget-payment-params"></a>

```json
{
  "source-address": "2N82m2L7p17jNzrnJKN4CTjuAApSYmRtPe2"
}

```

get-payment-params

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|source-address|[source-address](#schemasource-address)|true|none|Source address|

</section>
</section>

<section>
<h2 id="tocS_get-payment-response">get-payment-response</h2>
<!-- backwards compatibility -->
<a id="schemaget-payment-response"></a>
<a id="schema_get-payment-response"></a>
<a id="tocSget-payment-response"></a>
<a id="tocsget-payment-response"></a>

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

get-payment-response

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|payment-transaction-details|[transaction-details](#schematransaction-details)|true|none|none|
|escrow-transaction-details|[escrow-transaction-details](#schemaescrow-transaction-details)|true|none|none|

</section>
</section>

<section>
<h2 id="tocS_put-payment-params">put-payment-params</h2>
<!-- backwards compatibility -->
<a id="schemaput-payment-params"></a>
<a id="schema_put-payment-params"></a>
<a id="tocSput-payment-params"></a>
<a id="tocsput-payment-params"></a>

```json
{
  "fully-signed-pay-transaction": "02000000000101fea70fbb492e7814ce3a31639c64a7819a95ee43580867cc751a3e83438289fb01000000232200200818c20455783996fd6c4d753751f4c3f49d9a474fed9a65c3ddb31a48afa7f8ffffffff0200e1f5050000000017a9144f22a11a711f126454fd24e9d589529ed592c93387b800a3350000000017a914e90e71bb600bb67ac5c67b0f512c521dc1d7eceb87030047304402206f67c13ae9c8ad897090f8660993adabec91c1a221558f7a0fd46fe5054d911d02205c28139ab369be614588923742ea9b3922701240c1e19ab27983a1755ca8653c01255121032cea817f794f4afa7061a1187bb8108bfff0096c61f69468ce800629cebec87951ae00000000",
  "partially-signed-escrow-transaction": "0200000001fea70fbb492e7814ce3a31639c64a7819a95ee43580867cc751a3e83438289fb00000000b400483045022100c37d9f6bef9575be912e1963815f50efaa269779d7edcc72333044f14b3c52500220785d9bb8636125e30ca20a3b300b02f9566b33e0d5610b49393187a9c9ea603d01004c675241041256ae5095da25836abbe95f06f54ec27a745d66df3baa98f7ca1c54bfd12e26b279680220f779d059c3f168a7bbbc35344f5ff82b58d8329e920daba2586b3121032cea817f794f4afa7061a1187bb8108bfff0096c61f69468ce800629cebec87952aeffffffff0200c2eb0b0000000017a9144f22a11a711f126454fd24e9d589529ed592c93387dc13ae2f0000000017a914119cd81d4e3c2fab3e7edb5556ddaf5d1df893248700000000"
}

```

put-payment-params

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|fully-signed-pay-transaction|[fully-signed-pay-transaction](#schemafully-signed-pay-transaction)|false|none|A fully, user signed, pay transaction|
|partially-signed-escrow-transaction|[partially-signed-escrow-transaction](#schemapartially-signed-escrow-transaction)|true|none|User signed escrow transaction (1 out of 2)|

</section>
</section>

<section>
<h2 id="tocS_raw-pay-transaction">raw-pay-transaction</h2>
<!-- backwards compatibility -->
<a id="schemaraw-pay-transaction"></a>
<a id="schema_raw-pay-transaction"></a>
<a id="tocSraw-pay-transaction"></a>
<a id="tocsraw-pay-transaction"></a>

```json
"0200000001fea70fbb492e7814ce3a31639c64a7819a95ee43580867cc751a3e83438289fb0100000000ffffffff0200e1f5050000000017a9144f22a11a711f126454fd24e9d589529ed592c93387b800a3350000000017a914e90e71bb600bb67ac5c67b0f512c521dc1d7eceb8700000000"

```

raw-payment-transaction

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|raw-payment-transaction|string|false|none|A raw payment transaction from the given user's address|

</section>
</section>

<section>
<h2 id="tocS_fully-signed-pay-transaction">fully-signed-pay-transaction</h2>
<!-- backwards compatibility -->
<a id="schemafully-signed-pay-transaction"></a>
<a id="schema_fully-signed-pay-transaction"></a>
<a id="tocSfully-signed-pay-transaction"></a>
<a id="tocsfully-signed-pay-transaction"></a>

```json
"02000000000101fea70fbb492e7814ce3a31639c64a7819a95ee43580867cc751a3e83438289fb01000000232200200818c20455783996fd6c4d753751f4c3f49d9a474fed9a65c3ddb31a48afa7f8ffffffff0200e1f5050000000017a9144f22a11a711f126454fd24e9d589529ed592c93387b800a3350000000017a914e90e71bb600bb67ac5c67b0f512c521dc1d7eceb87030047304402206f67c13ae9c8ad897090f8660993adabec91c1a221558f7a0fd46fe5054d911d02205c28139ab369be614588923742ea9b3922701240c1e19ab27983a1755ca8653c01255121032cea817f794f4afa7061a1187bb8108bfff0096c61f69468ce800629cebec87951ae00000000"

```

fully-signed-pay-transaction

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|fully-signed-pay-transaction|string|false|none|A fully, user signed, pay transaction|

</section>
</section>

<section>
<h2 id="tocS_raw-escrow-transaction">raw-escrow-transaction</h2>
<!-- backwards compatibility -->
<a id="schemaraw-escrow-transaction"></a>
<a id="schema_raw-escrow-transaction"></a>
<a id="tocSraw-escrow-transaction"></a>
<a id="tocsraw-escrow-transaction"></a>

```json
"0200000001fea70fbb492e7814ce3a31639c64a7819a95ee43580867cc751a3e83438289fb0000000000ffffffff0200c2eb0b0000000017a9144f22a11a711f126454fd24e9d589529ed592c93387dc13ae2f0000000017a914119cd81d4e3c2fab3e7edb5556ddaf5d1df893248700000000"

```

raw-escrow-transaction

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|raw-escrow-transaction|string|false|none|A raw escrow transaction from the user's smart-credit address|

</section>
</section>

<section>
<h2 id="tocS_partially-signed-escrow-transaction">partially-signed-escrow-transaction</h2>
<!-- backwards compatibility -->
<a id="schemapartially-signed-escrow-transaction"></a>
<a id="schema_partially-signed-escrow-transaction"></a>
<a id="tocSpartially-signed-escrow-transaction"></a>
<a id="tocspartially-signed-escrow-transaction"></a>

```json
"0200000001fea70fbb492e7814ce3a31639c64a7819a95ee43580867cc751a3e83438289fb00000000b400483045022100c37d9f6bef9575be912e1963815f50efaa269779d7edcc72333044f14b3c52500220785d9bb8636125e30ca20a3b300b02f9566b33e0d5610b49393187a9c9ea603d01004c675241041256ae5095da25836abbe95f06f54ec27a745d66df3baa98f7ca1c54bfd12e26b279680220f779d059c3f168a7bbbc35344f5ff82b58d8329e920daba2586b3121032cea817f794f4afa7061a1187bb8108bfff0096c61f69468ce800629cebec87952aeffffffff0200c2eb0b0000000017a9144f22a11a711f126454fd24e9d589529ed592c93387dc13ae2f0000000017a914119cd81d4e3c2fab3e7edb5556ddaf5d1df893248700000000"

```

partially-signed-escrow-transaction

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|partially-signed-escrow-transaction|string|false|none|User signed escrow transaction (1 out of 2)|

</section>
</section>

<section>
<h2 id="tocS_tx-output">tx-output</h2>
<!-- backwards compatibility -->
<a id="schematx-output"></a>
<a id="schema_tx-output"></a>
<a id="tocStx-output"></a>
<a id="tocstx-output"></a>

```json
{
  "address": "2N82m2L7p17jNzrnJKN4CTjuAApSYmRtPe2",
  "amount": 1000000
}

```

tx-output

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|address|[destination-address](#schemadestination-address)|true|none|Destination address|
|amount|integer|true|none|Amount to send to the specific address|

</section>
</section>

<section>
<h2 id="tocS_transaction-details">transaction-details</h2>
<!-- backwards compatibility -->
<a id="schematransaction-details"></a>
<a id="schema_transaction-details"></a>
<a id="tocStransaction-details"></a>
<a id="tocstransaction-details"></a>

```json
{
  "coin-type": "BTC",
  "fee": 1000,
  "outputs": [
    {
      "address": "2N82m2L7p17jNzrnJKN4CTjuAApSYmRtPe2",
      "amount": 1000000
    }
  ]
}

```

transaction-details

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|coin-type|[coin-type-with-btc-example](#schemacoin-type-with-btc-example)|true|none|none|
|fee|[fee](#schemafee)|true|none|transaction fee/gas price|
|outputs|[[tx-output](#schematx-output)]|true|none|none|

</section>
</section>

<section>
<h2 id="tocS_escrow-transaction-details">escrow-transaction-details</h2>
<!-- backwards compatibility -->
<a id="schemaescrow-transaction-details"></a>
<a id="schema_escrow-transaction-details"></a>
<a id="tocSescrow-transaction-details"></a>
<a id="tocsescrow-transaction-details"></a>

```json
{
  "coin-type": "BTC",
  "source-address": "2Mx1EM2Bsd8qzV2yFv3XxemKaqW7ZfCKAFN",
  "raw-escrow-transaction": "0200000001fea70fbb492e7814ce3a31639c64a7819a95ee43580867cc751a3e83438289fb0000000000ffffffff0200c2eb0b0000000017a9144f22a11a711f126454fd24e9d589529ed592c93387dc13ae2f0000000017a914119cd81d4e3c2fab3e7edb5556ddaf5d1df893248700000000"
}

```

escrow-transaction-details

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|coin-type|[coin-type-with-btc-example](#schemacoin-type-with-btc-example)|true|none|none|
|source-address|[smart-credit-address](#schemasmart-credit-address)|true|none|The multisig address (2 out of 2) created for the smart-credit|
|raw-escrow-transaction|[raw-escrow-transaction](#schemaraw-escrow-transaction)|true|none|A raw escrow transaction from the user's smart-credit address|

</section>
</section>

<section>
<h2 id="tocS_atomic-generic-info">atomic-generic-info</h2>
<!-- backwards compatibility -->
<a id="schemaatomic-generic-info"></a>
<a id="schema_atomic-generic-info"></a>
<a id="tocSatomic-generic-info"></a>
<a id="tocsatomic-generic-info"></a>

```json
{
  "api-version": "v1"
}

```

atomic-generic-info

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|api-version|[api-version](#schemaapi-version)|true|none|Current API version|

</section>
</section>

<section>
<h2 id="tocS_destination-address">destination-address</h2>
<!-- backwards compatibility -->
<a id="schemadestination-address"></a>
<a id="schema_destination-address"></a>
<a id="tocSdestination-address"></a>
<a id="tocsdestination-address"></a>

```json
"2N82m2L7p17jNzrnJKN4CTjuAApSYmRtPe2"

```

destination-address

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|destination-address|string|false|none|Destination address|

</section>
</section>

<section>
<h2 id="tocS_source-address">source-address</h2>
<!-- backwards compatibility -->
<a id="schemasource-address"></a>
<a id="schema_source-address"></a>
<a id="tocSsource-address"></a>
<a id="tocssource-address"></a>

```json
"2N82m2L7p17jNzrnJKN4CTjuAApSYmRtPe2"

```

source-address

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|source-address|string|false|none|Source address|

</section>
</section>

<section>
<h2 id="tocS_public-key">public-key</h2>
<!-- backwards compatibility -->
<a id="schemapublic-key"></a>
<a id="schema_public-key"></a>
<a id="tocSpublic-key"></a>
<a id="tocspublic-key"></a>

```json
"027a478fd4a7c3b8e43cfd2eaf0e439158b2b874dace2b598c585e88f67eeb8655"

```

public-key

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|public-key|string|false|none|Public key|

</section>
</section>

<section>
<h2 id="tocS_payment-id">payment-id</h2>
<!-- backwards compatibility -->
<a id="schemapayment-id"></a>
<a id="schema_payment-id"></a>
<a id="tocSpayment-id"></a>
<a id="tocspayment-id"></a>

```json
"d290f1ee-6c54-4b01-90e6-d701748f0300"

```

payment-id

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|payment-id|string(uuid)|false|none|Payment ID|

</section>
</section>

<section>
<h2 id="tocS_tx-id">tx-id</h2>
<!-- backwards compatibility -->
<a id="schematx-id"></a>
<a id="schema_tx-id"></a>
<a id="tocStx-id"></a>
<a id="tocstx-id"></a>

```json
"850b5f43b0b13837f2fea92818c73034c7337c963245eaa4eca8b78e00515fd0"

```

tx-id

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|tx-id|string(string)|false|none|Transaction ID|

</section>
</section>

<section>
<h2 id="tocS_smart-credit-balance">smart-credit-balance</h2>
<!-- backwards compatibility -->
<a id="schemasmart-credit-balance"></a>
<a id="schema_smart-credit-balance"></a>
<a id="tocSsmart-credit-balance"></a>
<a id="tocssmart-credit-balance"></a>

```json
0

```

smart-credit-balance

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|smart-credit-balance|number(double)|false|none|Balance in the smart credit address|

</section>
</section>

<section>
<h2 id="tocS_smart-credit-address">smart-credit-address</h2>
<!-- backwards compatibility -->
<a id="schemasmart-credit-address"></a>
<a id="schema_smart-credit-address"></a>
<a id="tocSsmart-credit-address"></a>
<a id="tocssmart-credit-address"></a>

```json
"2Mx1EM2Bsd8qzV2yFv3XxemKaqW7ZfCKAFN"

```

smart-credit-address

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|smart-credit-address|string|false|none|The multisig address (2 out of 2) created for the smart-credit|

</section>
</section>

<section>
<h2 id="tocS_balance-with-example">balance-with-example</h2>
<!-- backwards compatibility -->
<a id="schemabalance-with-example"></a>
<a id="schema_balance-with-example"></a>
<a id="tocSbalance-with-example"></a>
<a id="tocsbalance-with-example"></a>

```json
102340000

```

balance-with-example

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|balance-with-example|any|false|none|none|

allOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[smart-credit-balance](#schemasmart-credit-balance)|false|none|Balance in the smart credit address|

and

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|any|false|none|none|

</section>
</section>

<section>
<h2 id="tocS_transaction-status">transaction-status</h2>
<!-- backwards compatibility -->
<a id="schematransaction-status"></a>
<a id="schema_transaction-status"></a>
<a id="tocStransaction-status"></a>
<a id="tocstransaction-status"></a>

```json
"PENDING-CONFIRMATIONS"

```

Transaction status in ATOMIC

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Transaction status in ATOMIC|

<section>

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|PENDING-OTHER-DEAL-PARTIES|
|*anonymous*|PENDING-CONFIRMATIONS|
|*anonymous*|PENDING-ESCROW-ACTIVATION|
|*anonymous*|PENDING-ESCROW-CONFIRMATIONS|
|*anonymous*|CONFIRMED|

</section>

</section>
</section>

<section>
<h2 id="tocS_api-version">api-version</h2>
<!-- backwards compatibility -->
<a id="schemaapi-version"></a>
<a id="schema_api-version"></a>
<a id="tocSapi-version"></a>
<a id="tocsapi-version"></a>

```json
"v1"

```

api-version

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|api-version|string|false|none|Current API version|

</section>
</section>

<section>
<h2 id="tocS_fee">fee</h2>
<!-- backwards compatibility -->
<a id="schemafee"></a>
<a id="schema_fee"></a>
<a id="tocSfee"></a>
<a id="tocsfee"></a>

```json
1000

```

transaction fee/gas price

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|integer|false|none|transaction fee/gas price|

</section>
</section>

<section>
<h2 id="tocS_coin-type">coin-type</h2>
<!-- backwards compatibility -->
<a id="schemacoin-type"></a>
<a id="schema_coin-type"></a>
<a id="tocScoin-type"></a>
<a id="tocscoin-type"></a>

```json
"BTC"

```

coin-type

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|coin-type|string|false|none|coin type|

<section>

#### Enumerated Values

|Property|Value|
|---|---|
|coin-type|BTC|
|coin-type|ETH|
|coin-type|ETC|
|coin-type|BCH|

</section>

</section>
</section>

<section>
<h2 id="tocS_coin-type-with-btc-example">coin-type-with-btc-example</h2>
<!-- backwards compatibility -->
<a id="schemacoin-type-with-btc-example"></a>
<a id="schema_coin-type-with-btc-example"></a>
<a id="tocScoin-type-with-btc-example"></a>
<a id="tocscoin-type-with-btc-example"></a>

```json
"BTC"

```

coin-type-with-btc-example

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|coin-type-with-btc-example|any|false|none|none|

allOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[coin-type](#schemacoin-type)|false|none|coin type|

and

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|any|false|none|none|

</section>
</section>

<section>
<h2 id="tocS_coin-type-with-eth-example">coin-type-with-eth-example</h2>
<!-- backwards compatibility -->
<a id="schemacoin-type-with-eth-example"></a>
<a id="schema_coin-type-with-eth-example"></a>
<a id="tocScoin-type-with-eth-example"></a>
<a id="tocscoin-type-with-eth-example"></a>

```json
"ETH"

```

coin-type-with-eth-example

<section>

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|coin-type-with-eth-example|any|false|none|none|

allOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[coin-type](#schemacoin-type)|false|none|coin type|

and

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|any|false|none|none|

</section>
</section>

