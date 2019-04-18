---
title: Wallet to ATOMIC APIs
language_tabs:
  - shell: cURL
toc_footers:
- <a href='https://www.atomic.org'>Visit ATOMIC Home Page</a>
includes:
  []
search: true
highlight_theme: darkula
headingLevel: 3

---

<!-- Generator: Widdershins v3.6.6 -->

# Wallet APIs

This section details the APIs from an ATOMIC powered wallet towards ATOMIC P2P collateral guarantees service

## User Management

User management related APIs

### Register New User


Register user with ATOMIC - creating his Private Collateral addresses

`PUT /api/general/v1/users/{user-id}`

<aside class="info">
The refund address will be used for cases in which small remainder is left after escrow activation
</aside>

> Body parameters

```json
{
    "coin-type": "BTC",
    "public-key": "027a478fd4a7c3b8e43cfd2eaf0e439158b2b874dace2b598c585e88f67eeb8655",
    "refund-address": "2NF1qc9L4j1tyfqzzLdRzRqoVftHznSeC6K"
}
```

> Code samples

```shell
curl -request PUT https://atomic.org/api/general/v1/users/{user-id} \
-H 'Content-Type: application/json' \
--data '{"public-key": "027a478fd4a7c3b8e43cfd2eaf0e439158b2b874dace2b598c585e88f67eeb8655", \
         "refund-address": "2NF1qc9L4j1tyfqzzLdRzRqoVftHznSeC6K"}'
```

> Example responses

> 201 Response

```json
{
    "primary-private-collateral-info":{
        "coin-type": "BTC",
        "atomic-pub-key":"037260dc3a7ab99fb5b51e035630a5dfb2187b90af02684dd5adb724f6dd28143a",
        "private-collateral-address":"2N3TpkuzGdCNYajPsbePpfdzQmKX4JT48SH",
        "private-collateral-redeem-script" : "5221037260dc3a7ab99fb5b51e035630a5dfb2187b90af02684dd5adb724f6dd28143a2103f2771401483f8f3d969d74281df0bc215e71c61bc4ec53df5b76420850ac0eb052ae",
        "private-collateral-script-pub-key":"a9147013460351b8baac0de0692530294bab3791c99d87"
        },
    "secondary-private-collateral-info":{
        "coin-type": "BTC",
        "atomic-pub-key":"022e74f3b11e5ada6753c402b87034579ebb21a5bd60bdcdc79f6f4f87c648ecb1",
        "private-collateral-address":"2NChgDFvx946rELsmfuzBWCh7L5SPsbY7y3",
        "private-collateral-redeem-script" : "5221022e74f3b11e5ada6753c402b87034579ebb21a5bd60bdcdc79f6f4f87c648ecb12103f2771401483f8f3d969d74281df0bc215e71c61bc4ec53df5b76420850ac0eb052ae",
        "private-collateral-script-pub-key":"a914d56b1867f1e93c800a775886eb961e8284f3b60887"
        },
}
```

#### Return Codes
Status|Meaning|Description
---|---|---
201 | OK  | User created succesfully
400 | Bad Request | Invalid request or conflict with existing user
409 | Already Exist | User already exists

### Get User Information

Retrieves existing information of a specific user

`GET /api/general/v1/users/{user-id}`

> Code samples

```shell
curl -request GET https://atomic.org/api/general/v1/users/{user-id}
```

> Example responses

> 200 Response

```json
{
   "primary-private-collateral-info":{
        "coin-type": "BTC",
        "atomic-pub-key":"037260dc3a7ab99fb5b51e035630a5dfb2187b90af02684dd5adb724f6dd28143a",
        "private-collateral-address":"2N3TpkuzGdCNYajPsbePpfdzQmKX4JT48SH",
        "private-collateral-redeem-script" : "5221037260dc3a7ab99fb5b51e035630a5dfb2187b90af02684dd5adb724f6dd28143a2103f2771401483f8f3d969d74281df0bc215e71c61bc4ec53df5b76420850ac0eb052ae",
        "private-collateral-script-pub-key":"a9147013460351b8baac0de0692530294bab3791c99d87"
    },
   "secondary-private-collateral-info":{
        "coin-type": "BTC",
        "atomic-pub-key":"022e74f3b11e5ada6753c402b87034579ebb21a5bd60bdcdc79f6f4f87c648ecb1",
        "private-collateral-address":"2NChgDFvx946rELsmfuzBWCh7L5SPsbY7y3",
        "private-collateral-redeem-script" : "5221022e74f3b11e5ada6753c402b87034579ebb21a5bd60bdcdc79f6f4f87c648ecb12103f2771401483f8f3d969d74281df0bc215e71c61bc4ec53df5b76420850ac0eb052ae",
        "private-collateral-script-pub-key":"a914d56b1867f1e93c800a775886eb961e8284f3b60887"
    },
  "private-collateral-balance": "102340000",
  "inflight-transactions": [
    {
      "coin-type": "BTC",
      "payment-id": "d290f1ee-6c54-4b01-90e6-d701748f0300",
      "transaction-id": "850b5f43b0b13837f2fea92818c73034c7337c963245eaa4eca8b78e00515fd0",
      "status": "PENDING-CONFIRMATIONS"
    }
  ]
}
```

#### Return Codes
Status|Meaning|Description
---|---|---
200 | OK | OK
404 | Not Found | User not found

### Unregister User

Unregister the user from ATOMIC

`DELETE /api/general/v1/users/{user-id}`

<aside class="notice">
Unregister will not be allowed as long as there are inflight transactions, or funds in the private-collateral address
</aside>

> Code samples

```shell
curl -request DELETE https://atomic.org/api/general/v1/users/{user-id}
```

#### Return Codes
Status|Meaning|Description
---|---|---
200 | OK | OK
403| Forbidden | Unregister not allowed
404 | Not Found | User not found


## Unlock Operations

APIs for Unlocking funds from the user's primary private collateral

<!--
### Create On-Demand Unlock
Create an on-demand unlock transaction from the user's private-collateral address

`POST /api/general/v1/users/{user-id}/unlock`

<aside class = "notice">
Amount 0 will create an unlock transaction from all available funds
</aside>

<aside class="warning">
NOTE: An on demand unlock cannot be processed during ongoing payment transactions
</aside>


> Body parameters

```json
{
    "amount": 498000000
}
```

> Code samples

```shell
curl -request POST https://atomic.org/api/general/v1/users/{user-id}/unlock \
 -H 'Content-Type: application/json' --data '{"amount": 498000000 }'
```

> Example responses

> 200 Response

```json
{
  "raw-transaction": "02000000018e62016f5fe7fad1f7f343cdf900f8c6aad4cd1bef9c2a274334291c3999551c0000000000feffffff0118c69a3b0000000017a91455f415046dfb9e578426fbcbfb480a66b0782480876e000000"
}
```

#### Return Codes
Status|Meaning|Description
---|---|---
200 | OK | On demand unlock transaction created
403 | Forbidden | Cannot perform on-demand unlock at this time
404 | Not Found | User not found
-->

### Submit An On-Demand Unlock

Perform an unlock of funds from the user's private-collateral address.

Should supply the unlock transaction, signed (1 of 2) by the wallet.

`PUT /api/general/v1/users/{user-id}/unlock/perform`

<aside class="warning">
NOTE: An on demand unlock cannot be processed during ongoing transactions
</aside>


> Body parameters

```json
{
    "coin-type": "BTC",
    "partially-signed-unlock-tx": "02000000018e62016f5fe7fad1f7f343cdf900f8c6aad4cd1bef9c2a274334291c3999551c00000000b400483045022100de328bf0550164f0b6f1ae3bae78c6d417140808fa82f0da67be00cd8df41816022024b2fc2182789488fcca4984e7af3123bd3e680e4f4be8eaeb3e547f24ca31ea01004c675241043af1ba0a3429d7890d91a7f9ff280b7130c3b5edd1c02613e9f770a05ec1977a76646d7223ff17f3881f07e08a15f31cb23e79a7871202e52c678f5b9af6823021033f1654d3187087ad5f7f220814c94232945825663d55431345aca8a82746780552aefeffffff0118c69a3b0000000017a91455f415046dfb9e578426fbcbfb480a66b0782480876e000000"
}
```

> Code samples

```shell
curl -request PUT https://atomic.org/api/general/v1/users/{user-id}/unlock/perform 
-H 'Content-Type: application/json' --data '{"coin-type": "BTC", "partially-signed-unlock-tx": \ "02000000018e62016f5fe7fad1f7f343cdf900f8c6aad4cd1bef9c2a274334291c3999551c00000000b400483045022100de328bf0550164f0b6f1ae3bae78c6d417140808fa82f0da67be00cd8df41816022024b2fc2182789488fcca4984e7af3123bd3e680e4f4be8eaeb3e547f24ca31ea01004c675241043af1ba0a3429d7890d91a7f9ff280b7130c3b5edd1c02613e9f770a05ec1977a76646d7223ff17f3881f07e08a15f31cb23e79a7871202e52c678f5b9af6823021033f1654d3187087ad5f7f220814c94232945825663d55431345aca8a82746780552aefeffffff0118c69a3b0000000017a91455f415046dfb9e578426fbcbfb480a66b0782480876e000000"}'
```

#### Return Codes
Status|Meaning|Description
--------- |  ----------- | -----------
201| Created|Unlock command recieved
400| Bad Request | Invalid input
403| Forbidden|Cannot perform unlock at this time
404 |Not Found | User not found
409| Conflict | Another unlock already in progress


### Create Timed Unlock

Request ATOMIC to sign the (independently created) timed unlock transaction from the user's Private Coilateral address at the given lock time

`POST /api/general/v1/users/{user-id}/unlock/timed`

<aside class="info">
The fully signed transaction (the response), should be stored by the wallet, and submitted when the lock-time arrives
</aside>

<aside class="warning">
Payment transactions will not be allowed near the target unlock time
</aside>

> Body parameters

```json
{
    "coin-type": "BTC",
    "partially-signed-unlock-tx": "020000000001016e069dc051125daa13babacc05cf1005d78af655a53f86749071246e8e5e735a0000000023220020d2a1b05864a583d696e155d9de3772dde728e059964443091f156c455d3dd835feffffff0118c69a3b0000000017a91470da9b3ae8884cedd69dd800c46b15fdeb840f3c8704004730440220075195504bddb720ffdaf3b76d4cd62e08503386466700c170864caf75bf5e220220597b9f6017d51784948f2c7835f50bfc2403850bf80aeea90fd359b26f87f6620100475221037a2ce0416838155e9e6a486687e5bad626e5b6930796351dc6c65b2aa26805b82102cfee084d838e114e2b6489e82e77ebd6fffc84046a2656ca6b3367c0f6e5173552ae6e000000"
}
```

> Code samples

```shell
curl -request POST https://atomic.org/api/general/v1/users/{user-id}/unlock/timed \
-H 'Content-Type: application/json' --data '{ \
      "coin-type": "BTC", \
      "partially-signed-tx": "020000000001016e069dc051125daa13babacc05cf1005d78af655a53f86749071246e8e5e735a0000000023220020d2a1b05864a583d696e155d9de3772dde728e059964443091f156c455d3dd835feffffff0118c69a3b0000000017a91470da9b3ae8884cedd69dd800c46b15fdeb840f3c8704004730440220075195504bddb720ffdaf3b76d4cd62e08503386466700c170864caf75bf5e220220597b9f6017d51784948f2c7835f50bfc2403850bf80aeea90fd359b26f87f6620100475221037a2ce0416838155e9e6a486687e5bad626e5b6930796351dc6c65b2aa26805b82102cfee084d838e114e2b6489e82e77ebd6fffc84046a2656ca6b3367c0f6e5173552ae6e000000" \
}'  
```

> Example responses

> 200 Response

```json
{
      "fully-signed-tx": "020000000001016e069dc051125daa13babacc05cf1005d78af655a53f86749071246e8e5e735a0000000023220020d2a1b05864a583d696e155d9de3772dde728e059964443091f156c455d3dd835feffffff0118c69a3b0000000017a91470da9b3ae8884cedd69dd800c46b15fdeb840f3c87040047304402202a78c8d8e295fd20e00ed9315edc890c975d2439b5de7961662d62f44c3680c00220074e066490b6ecd0ba98fc4dc93cb6161979424b7c7b2ec20ce70a12edc10a8d014730440220075195504bddb720ffdaf3b76d4cd62e08503386466700c170864caf75bf5e220220597b9f6017d51784948f2c7835f50bfc2403850bf80aeea90fd359b26f87f66201475221037a2ce0416838155e9e6a486687e5bad626e5b6930796351dc6c65b2aa26805b82102cfee084d838e114e2b6489e82e77ebd6fffc84046a2656ca6b3367c0f6e5173552ae6e000000"
}
```

#### Return Codes
Status|Meaning|Description
---|---|---
201| Created | Unlock command received
400| Bad Request | Inavlid input
403| Forbidden | Cannot perform unlock, earlier unlock/transaction is already in progress
404| Not Found | User not found
409| Conflict | Already exists 

## Payments

APIs for confirming payments initiated from various financial services

ATOMIC Payment is comprised of four transactions - 
three escrow transactions, and a single payment transaction

### Prepare Payment

This is the first phase of the Payment confirmation process.

In this phase - the wallet gets the "Primary Escrow Transaction"

`GET /api/general/v1/users/{user-id}/payments/{payment-id}/prepare`

> Code samples

```shell
curl -request GET https://atomic.org/api/general/v1/users/{user-id}/payments/{payment-id}/prepare 
```

> Example responses

> 200 Response

```json
{
    "primary-escrow-details": {
        "coin-type": "BTC",
        "raw-tx": "0200000001d17c10b7486d1a6da4128802f533a361d4dd160152752c7c585827e5c05af7c90000000000ffffffff0212dfcd1d0000000017a914779208bacd21597a2fbed8f739d384bf7663600a87dcce9cd00000000017a914c437ad520cc819d7e6bd480c739139b0b33226d88700000000"
    }
}
```

#### Return Codes
Status|Meaning|Description
---|---|---
200| OK | OK
400| Bad Request | Error - see body for details
404| Not Found | User/Payment not found
409| Conflict | Payment already performed


### Get Payment Details

This is the second phase of the Payment confirmation process.

In this phase - the wallet provides the signed transaction from the first phase (prepare), and gets all the required information for payment confirmation.

`POST /api/general/v1/users/{user-id}/payments/{payment-id}`

<aside class="success">
ATOMIC will provide the raw escrow transactions, as well as the required details for the pay transaction
</aside>

<aside class="notice">
The Input transaction for the secondary & return escrow transactions are the primary escrow
</aside>

> Body parameters

```json
{
    "partially-signed-primary-escrow-tx": "02000000000101d17c10b7486d1a6da4128802f533a361d4dd160152752c7c585827e5c05af7c90000000023220020ba1535217938a9c8ff2deb3d3d5f08d07e882b0f6b969e4586b55e031b17c9abffffffff0212dfcd1d0000000017a914779208bacd21597a2fbed8f739d384bf7663600a87dcce9cd00000000017a914c437ad520cc819d7e6bd480c739139b0b33226d887040047304402202aae02aa182a13c54ff8bda594882899452d77df76481be22101c4fc61769bde02201b113db024198719a3805beefb55bf9a6009dc34e094a7df443980684d8d4d3e010047522103720bb3b7ecbf2a5c14fafe380a777c17199b6b8df2d7850ab4de4653bdb115532103d55a1526cd6aad84103722e952ade3ac7b969c5a576ef74ed367f814a2173c5452ae00000000"
}
```


> Code samples

```shell
curl -request POST https://atomic.org/api/general/v1/users/{user-id}/payments/{payment-id} \
-H 'Content-Type: application/json' --data '{ \
    "partially-signed-primary-escrow-tx": "02000000000101d17c10b7486d1a6da4128802f533a361d4dd160152752c7c585827e5c05af7c90000000023220020ba1535217938a9c8ff2deb3d3d5f08d07e882b0f6b969e4586b55e031b17c9abffffffff0212dfcd1d0000000017a914779208bacd21597a2fbed8f739d384bf7663600a87dcce9cd00000000017a914c437ad520cc819d7e6bd480c739139b0b33226d887040047304402202aae02aa182a13c54ff8bda594882899452d77df76481be22101c4fc61769bde02201b113db024198719a3805beefb55bf9a6009dc34e094a7df443980684d8d4d3e010047522103720bb3b7ecbf2a5c14fafe380a777c17199b6b8df2d7850ab4de4653bdb115532103d55a1526cd6aad84103722e952ade3ac7b969c5a576ef74ed367f814a2173c5452ae00000000" \
}'
```

> Example responses

> 200 Response

```json
{
  "payment-type": FULL,
  "pay-details": {
    "coin-type": "BTC",
    "fee": 1000,
    "num-outputs": 1,
    "target-address": "2MsJzTLwm1bQJkbC7tjqVUaJ8jF5FBByrNn",
    "amount": "500000000",
    "second-target-address": null,
    "second-amount": "0",
  },
  "primary-escrow-details": {
    "coin": "BTC",
    "input-info":{
        "address":"2NB8j8Pcv2RoiGFdmLXnWP3n7JHw77zj3G5",
        "redeemScript": "522103720bb3b7ecbf2a5c14fafe380a777c17199b6b8df2d7850ab4de4653bdb115532103d55a1526cd6aad84103722e952ade3ac7b969c5a576ef74ed367f814a2173c5452ae"
    },
    "num-outputs":1,
    "target-address": "2N49TLjp5DiCJ9pMhQ4qe5niPen3CREq3W5",
    "amount": "500031250",
    "second-target-address": null,
    "second-amount": "0",
    "fee": "31250",
    "raw-tx": null
  },
  "secondary-escrow-details": {
    "coin": "BTC",
    "input-info": {
        "address": "2N49TLjp5DiCJ9pMhQ4qe5niPen3CREq3W5",
        "redeemScript": "522103286234c63a8db5b66d089976468fade14f15d44df5c19cdd80694e74f5139a502103d55a1526cd6aad84103722e952ade3ac7b969c5a576ef74ed367f814a2173c5452ae",
        "inputTxId":"e22fa048ec9060eb0dc65dd6d2bac8debbffa47703d2d7ba2a31c445ee053263",
        "vout": 0,
        "amount": "500031250"
    },
    "num-outputs": 1,
    "target-address": "2MsJzTLwm1bQJkbC7tjqVUaJ8jF5FBByrNn",
    "amount": "500000000",
    "second-target-address": null,
    "second-amount": "0",
    "fee": "31250",
    "raw-tx": "0200000001633205ee45c4312abad7d20377a4ffbbdec8bad2d65dc60deb6090ec48a02fe20000000000ffffffff010065cd1d0000000017a91400b65de04f3a8dc0be83d14749ccee3c99d7c7858700000000"
  },
  "return-escrow-details": {
    "coin": "BTC",
    "input_info": {
        "address": "2N49TLjp5DiCJ9pMhQ4qe5niPen3CREq3W5",
        "redeemScript": "522103286234c63a8db5b66d089976468fade14f15d44df5c19cdd80694e74f5139a502103d55a1526cd6aad84103722e952ade3ac7b969c5a576ef74ed367f814a2173c5452ae",
        "inputTxId": "e22fa048ec9060eb0dc65dd6d2bac8debbffa47703d2d7ba2a31c445ee053263",
        "vout": 0,
        "amount": "500031250"
    },
    "num-outputs": 1,
    "target-address": "2NB8j8Pcv2RoiGFdmLXnWP3n7JHw77zj3G5",
    "amount": "500000000",
    "second-target-address": null,
    "second-amount": "0",
    "fee": "31250",
    "raw-tx": "0200000001633205ee45c4312abad7d20377a4ffbbdec8bad2d65dc60deb6090ec48a02fe20000000000ffffffff010065cd1d0000000017a914c437ad520cc819d7e6bd480c739139b0b33226d88700000000"
  }
}
```

#### Return Codes
Status|Meaning|Description
---|---|---
200| OK | Payment found
400| Bad Request | Inavlid input
404| Not Found | User/Payment not found
409| Conflict | Payment already performed


### Confirm Payment

This is the third and last phase of the Payment confirmation process.

In this phase - the wallet submites all the requried transactions, and by doing so - confirms the payment.

`PUT /api/general/v1/users/{user-id}/payments/{payment-id}/confirm`


> Body parameters

```json
{
    "fully-signed-pay-tx": "020000000001013f2bf4dda666fba203bf134ff0089fa9bedd822ba0e4e3201cc951766435280600000000171600143042968fb09feccebab20f8d96d625272ffce6dfffffffff020065cd1d0000000017a91400b65de04f3a8dc0be83d14749ccee3c99d7c78587ee12380c0100000017a914614bb7051bf0802e01574760e594fdbd3505997787024730440220247c702c2ad6ea5e1bcac6b0fd91df33d49054ad802e5111eef59b265458dd5202204126ad02aaaef77304dccdf58deae311e4fa8a4e54757eadef1d99940d1145800121023f66527707b3b528e7075ad7942b5b0e63a174a24e4b7e8db333965ba0ceaf2400000000",
    "partially-signed-primary-escrow-tx": "02000000000101d17c10b7486d1a6da4128802f533a361d4dd160152752c7c585827e5c05af7c90000000023220020ba1535217938a9c8ff2deb3d3d5f08d07e882b0f6b969e4586b55e031b17c9abffffffff0212dfcd1d0000000017a914779208bacd21597a2fbed8f739d384bf7663600a87dcce9cd00000000017a914c437ad520cc819d7e6bd480c739139b0b33226d887040047304402202aae02aa182a13c54ff8bda594882899452d77df76481be22101c4fc61769bde02201b113db024198719a3805beefb55bf9a6009dc34e094a7df443980684d8d4d3e010047522103720bb3b7ecbf2a5c14fafe380a777c17199b6b8df2d7850ab4de4653bdb115532103d55a1526cd6aad84103722e952ade3ac7b969c5a576ef74ed367f814a2173c5452ae00000000",
    "partially-signed-secondary-escrow-tx": "02000000000101633205ee45c4312abad7d20377a4ffbbdec8bad2d65dc60deb6090ec48a02fe20000000023220020ebc41ba034dc92cc5e01f5becaa8c91d2d77f632f914f6be7c690ab60ed1d7baffffffff010065cd1d0000000017a91400b65de04f3a8dc0be83d14749ccee3c99d7c78587040047304402205d5d1b98a13c99da24dd3d2bb9e4da4aaf6c72eb102ed98c8da5986593e0a5fb02204e59b817fa69553e2e51f6d0e4e67ccab17c2be01fbdf8332a95ce2b21a15ca7010047522103286234c63a8db5b66d089976468fade14f15d44df5c19cdd80694e74f5139a502103d55a1526cd6aad84103722e952ade3ac7b969c5a576ef74ed367f814a2173c5452ae00000000",
    "partially-signed-return-escrow-tx": "02000000000101633205ee45c4312abad7d20377a4ffbbdec8bad2d65dc60deb6090ec48a02fe20000000023220020ebc41ba034dc92cc5e01f5becaa8c91d2d77f632f914f6be7c690ab60ed1d7baffffffff010065cd1d0000000017a914c437ad520cc819d7e6bd480c739139b0b33226d887040047304402202cec801f3066ad54015d9aaee7eb2da1ef31c6fe9fb8eea2956442cc15dd9ae3022076121c89b2834947bcd3e30040182dec3a85b0724947f9e9e28c43a3aa2dd5cb010047522103286234c63a8db5b66d089976468fade14f15d44df5c19cdd80694e74f5139a502103d55a1526cd6aad84103722e952ade3ac7b969c5a576ef74ed367f814a2173c5452ae00000000"
}
```

> Code samples

```shell
curl -request PUT https://atomic.org/api/general/v1/users/{user-id}/payments/{payment-id}/confirm \ 
-H 'Content-Type: application/json' --data '{ \
    "fully-signed-pay-tx": "020000000001013f2bf4dda666fba203bf134ff0089fa9bedd822ba0e4e3201cc951766435280600000000171600143042968fb09feccebab20f8d96d625272ffce6dfffffffff020065cd1d0000000017a91400b65de04f3a8dc0be83d14749ccee3c99d7c78587ee12380c0100000017a914614bb7051bf0802e01574760e594fdbd3505997787024730440220247c702c2ad6ea5e1bcac6b0fd91df33d49054ad802e5111eef59b265458dd5202204126ad02aaaef77304dccdf58deae311e4fa8a4e54757eadef1d99940d1145800121023f66527707b3b528e7075ad7942b5b0e63a174a24e4b7e8db333965ba0ceaf2400000000", \
    "partially-signed-primary-escrow-tx": "02000000000101d17c10b7486d1a6da4128802f533a361d4dd160152752c7c585827e5c05af7c90000000023220020ba1535217938a9c8ff2deb3d3d5f08d07e882b0f6b969e4586b55e031b17c9abffffffff0212dfcd1d0000000017a914779208bacd21597a2fbed8f739d384bf7663600a87dcce9cd00000000017a914c437ad520cc819d7e6bd480c739139b0b33226d887040047304402202aae02aa182a13c54ff8bda594882899452d77df76481be22101c4fc61769bde02201b113db024198719a3805beefb55bf9a6009dc34e094a7df443980684d8d4d3e010047522103720bb3b7ecbf2a5c14fafe380a777c17199b6b8df2d7850ab4de4653bdb115532103d55a1526cd6aad84103722e952ade3ac7b969c5a576ef74ed367f814a2173c5452ae00000000", \
    "partially-signed-secondary-escrow-tx": "02000000000101633205ee45c4312abad7d20377a4ffbbdec8bad2d65dc60deb6090ec48a02fe20000000023220020ebc41ba034dc92cc5e01f5becaa8c91d2d77f632f914f6be7c690ab60ed1d7baffffffff010065cd1d0000000017a91400b65de04f3a8dc0be83d14749ccee3c99d7c78587040047304402205d5d1b98a13c99da24dd3d2bb9e4da4aaf6c72eb102ed98c8da5986593e0a5fb02204e59b817fa69553e2e51f6d0e4e67ccab17c2be01fbdf8332a95ce2b21a15ca7010047522103286234c63a8db5b66d089976468fade14f15d44df5c19cdd80694e74f5139a502103d55a1526cd6aad84103722e952ade3ac7b969c5a576ef74ed367f814a2173c5452ae00000000", \
    "partially-signed-return-escrow-tx": "02000000000101633205ee45c4312abad7d20377a4ffbbdec8bad2d65dc60deb6090ec48a02fe20000000023220020ebc41ba034dc92cc5e01f5becaa8c91d2d77f632f914f6be7c690ab60ed1d7baffffffff010065cd1d0000000017a914c437ad520cc819d7e6bd480c739139b0b33226d887040047304402202cec801f3066ad54015d9aaee7eb2da1ef31c6fe9fb8eea2956442cc15dd9ae3022076121c89b2834947bcd3e30040182dec3a85b0724947f9e9e28c43a3aa2dd5cb010047522103286234c63a8db5b66d089976468fade14f15d44df5c19cdd80694e74f5139a502103d55a1526cd6aad84103722e952ade3ac7b969c5a576ef74ed367f814a2173c5452ae00000000"}'
```

#### Return Codes
Status|Meaning|Description
---|---|---
201| Created | Payment received 
400| Bad Request  | Inavlid input
404| Not Found  | User/Payment not found
409| Conflict  | Payment already performed 

## General Information

General information from ATOMIC P2P Collateral Guarantees Service

### Get ATOMIC Generic Information

Get general ATOMIC information

`GET /api/general/v1/info`

> Code samples

```shell
curl -request GET https://atomic.org/api/general/v1/info 
```

> Example responses

> 200 Response

```json
{
  "api-version": "v1"
}
```

#### Return Codes
Status|Meaning|Description
---|---|---
200 |OK | OK
400|Bad Request | Bad input parameter


## Trade Platform APIs
 
 This section details the APIs from an ATOMIC powered wallet to an ATOMIC powered P2P Trading Platform
 
### Create Order
 
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
 curl -request PUT https://atomic.org/api/trades/v1/users/{user-id}/orders/{order-id} \ 
 -H 'Content-Type: application/json' --data '{ \
     "source-coin": "BTC",        \
     "source-amount": 1010000000, \
     "destination-coin": "ETH",   \
     "destination-amount": 300500000000000000000, \
     "destination-address": "0xe8e49e84480edd95aaae50a340422cb30057963b", \
     "escrow-coin": "BTC",        \
     "escrow-amount": 1010000000, \
     "escrow-address": "2NF1qc9L4j1tyfqzzLdRzRqoVftHznSeC6K" \
     }'
 ```
 
#### Return Codes
 Status|Meaning|Description
 ---|---|---
 201 | Created | Order created succesfully 
 400 | Bad Request | Invalid input
 409 | Conflict | Order already exists


### Get Order Details

Retrieves the existing information for a specific order.

<aside class="success">
If the order is currently in an active deal(s)- will also return the payment ID(s)
</aside>

`GET /api/trades/v1/users/{user-id}/orders/{order-id}`

> Code samples

```shell
curl -request GET https://atomic.org/api/trades/v1/users/{user-id}/orders/{order-id} 
```

> Example responses

> 200 Response

```json
[
    {
        "order-id": "d290f1ee-6c54-4b01-90e6-d701748f0100",
        "user-id": "d290f1ee-6c54-4b01-90e6-d701748f0200",
        "status": "IN_DEAL",
        "payment-id": "d290f1ee-6c54-4b01-90e6-d701748f0300"
    }
]
```

#### Return Codes
Status|Meaning|Description
---|---|---
200 | OK | Order found
404 | Not Found | Order not found

### Cancel Order

Deletes a given order, if allowed

`DELETE /api/trades/v1/users/{user-id}/orders/{order-id}`

> Code samples

```shell
curl -request DELETE https://atomic.org/api/trades/v1/users/{user-id}/orders/{order-id}
```

#### Return Codes
Status|Meaning|Description
---|---|---
200 | OK | Order canceled
400 | Bad Request | Invalid input
403 | Forbidden | Order is currently being executed, unable to cancel 
404 | Not Found | Order not found

### Get Supported Coins

List of supported coins

<aside class="success">
By deafult, trade between all coins is supported
</aside>

`GET /api/trades/v1/coins`

> Code samples

```shell
curl -request GET https://atomic.org/api/trades/v1/coins'
```

> Example responses

> 200 Response

```json
{
   ["BTC", "LTC", "DASH", "BSV", "DOGE", "BCH", "ETC", "ETH"]
}
```

### Get Generic Info

Generic info from the P2P trading platform

`GET /api/trades/v1/info`

> Code samples

```shell
curl -request GET https://atomic.org/api/trades/v1/info'
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

## Credit Line Platforms APIs

This section details the APIs from an ATOMIC powered wallet to an ATOMIC powered Credit Line provider

### Request Credit Line

Request/Update Credit Line amount

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
curl -request PUT https://atomic.org/api/credit-line/v1/users/{user-id}/credits/{credit-id} \
-H 'Content-Type: application/json' --data '{ "coin-type": "BTC", "amount": 1050000000}'
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

#### Return Codes
Status|Meaning|Description
---|---|---
201 | Created | Credit Line updated - escrow required
400 | Bad Request | Invalid input
403 | Forbidden  | Credit Line is lower than existing
409 | Conflict | Credit Line up to date - no additional escrow required


### Get Credit Line Details

Gets the user's Credit Line information 

<aside class="success">
Returns a payment ID in case such is required
</aside>

`GET /api/credit-line/v1/users/{user-id}/credits/{credit-id}`

> Code samples

```shell
curl -request GET https://atomic.org/api/credit-line/v1/users/{user-id}/credits/{credit-id}
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

#### Return Codes
Status|Meaning|Description
---|---|---
200| OK | Credit Line found
404| Not Found | User/Credit Line not found


### Delete Credit Line

Request to remove a specific Credit Line

`DELETE /api/credit-line/v1/users/{user-id}/credits/{credit-id}`

> Code samples

```shell
curl -request DELETE https://atomic.org/api/credit-line/v1/users/{user-id}/credits/{credit-id}
```

#### Return Codes
Status|Meaning|Description|Schema|
---|---|---|---|
200| OK | Credit Line deleted succesfully
403| Forbidden | Credit Line not yet settled - delete not allowed
404| Not Found | User/Credit Line not found

### Get User Details

Gets the list of all lines of credit for a given user

`GET /api/credit-line/v1/users/{user-id}`

> Code samples

```shell
curl -request GET https://atomic.org/api/credit-line/v1/users/{user-id}
```

> Example responses

> 200 Response

```json
[
    {
        "user-id": "d290f1ee-6c54-4b01-90e6-d701748f0200",
        "amount": 1050000000,
        "coin-type": "BTC",
        "payment-id": "d290f1ee-6c54-4b01-90e6-d701748f0300"
    }
]
```

#### Return Codes
Status|Meaning|Description
---|---|---
200| OK | OK
404| Not Found | User not found


### Get Supported Coins

List of supported coins

`GET /api/credit-line/v1/coins`

> Code samples

```shell
curl -request GET https://atomic.org/api/credit-line/v1/coins
```

> Example responses

> 200 Response

```jsonß
{
    ["BTC", "LTC", "DASH", "BSV", "DOGE", "BCH", "ETC", "ETH"]
}
```

### Get Generic Info

Generic info from the Credit Line provider

`GET /api/credit-line/v1/info`

> Code samples

```shell
curl -request GET https://atomic.org/api/credit-line/v1/info
```

> Example responses

> 200 Response

```json
{
    "business-name": "Lifeline - Credit Lines",
    "business-id": "a450f1fa-6d54-4b01-90e6-d701748f0400",
    "api-version": "v1"
}
```

## Loan Platform APIs

This section details the APIs from an ATOMIC powered wallet to an ATOMIC powered P2P Loans Platform

### Request Loan

Request a new loan or update an existing loan

`PUT /api/loans/v1/borrowers/{user-id}/loans/{loan-id}`

> Body parameter

```json
{
    "user-id": "d290f1ee-6c54-4b01-90e6-d701748f0200",
    "coin-type": "BTC",
    "amount": 560000000,
    "destination-address": "2NF1qc9L4j1tyfqzzLdRzRqoVftHznSeC6K",
    "max-interest": 7.1,
    "loan-duration-in-days": 527
}
```

> Code samples

```shell
curl -request PUT https://atomic.org/api/loans/v1/borrowers/{user-id}/loans/{loan-id} \
-H 'Content-Type: application/json' --data '{ \
         "user-id": "d290f1ee-6c54-4b01-90e6-d701748f0200", \
         "coin-type": "BTC", \
         "amount": 560000000, \
         "destination-address": "2NF1qc9L4j1tyfqzzLdRzRqoVftHznSeC6K", \
         "max-interest": 7.1, \
         "loan-duration-in-days": 527 \
}'
```

#### Return Codes
Status|Meaning|Description
---|---|---
201| Created | Loan request created
400| Bad Request | Invalid input
403| Forbidden | Can't update loan to an amount lower than already received
409| Conflict | Loan already exists


### Get Loan Details

Gets the user's loan information

`GET /api/loans/v1/borrowers/{user-id}/loans/{loan-id}`

> Code samples

```shell
curl -request GET https://atomic.org/api/loans/v1/borrowers/{user-id}/loans/{loan-id}
```

> Example responses

> 200 Response

```json
{
    "user-id": "d290f1ee-6c54-4b01-90e6-d701748f0200",
    "loan-id": "d290f1ee-6c54-4b01-90e6-d701748f0500",
    "coin-type": "BTC",
    "max-interest-rate": 7.1,
    "requested-amount": 560000000,
    "filled-amount": 420000000,
    "loan-details":
        {
            "coin-type": "BTC",
            "amount": 420000000,
            "amount-due": 43500000,
            "interest": 6.9,
            "duration-in-days": 527,
            "expiration-date": 1595697538
        }
}
```


#### Return Codes
Status|Meaning|Description
---|---|---
200| OK | Loan found
404| Not Found | User/Loan not found


### Delete Loan

Request to delete a user's loan

`DELETE /api/loans/v1/borrowers/{user-id}/loans/{loan-id}`

> Code samples

```shell
curl -request DELETE https://atomic.org/api/loans/v1/borrowers/{user-id}/loans/{loan-id}
```

#### Return Codes
Status|Meaning|Description
---|---|---
200| OK | Loan deleted successfully
403| Forbidden | Loan is in progress, unable to delete
404| Not Found | User/Loan not found

### List Loans

Gets the user's loans information

`GET /api/loans/v1/borrowers/{user-id}`

> Code samples

```shell
curl -request GET https://atomic.org/api/loans/v1/borrowers/{user-id}
```

> Example responses

> 200 Response

```json
{
    "user-id": "d290f1ee-6c54-4b01-90e6-d701748f0200",
    "loan-id": "d290f1ee-6c54-4b01-90e6-d701748f0500",
    "coin-type": "BTC",
    "max-interest-rate": 7.1,
    "requested-amount": 560000000,
    "filled-amount": 420000000,
    "num-active-loans": 1,
    "active-loans": [
            {
            "coin-type": "BTC",
            "amount": 420000000,
            "amount-due": 43500000,
            "interest": 6.9,
            "duration-in-days": 527,
            "expiration-date": 1595697538
            }
        ]
    }
```

#### Return Codes
Status|Meaning|Description
---|---|---
200| OK | OK
404| Not Found | User not found

### Create Investment

Create a new investment/Update an existing investment

`PUT /api/loans/v1/investors/{user-id}/investments/{investment-id}`

> Body parameter

```json
{
    "coin-type": "BTC",
    "amount": 1000000000,
    "min-interest": 5.4,
    "max-loan-duration-in-days": 527,
    "expiration-date": 1737307138,
    "destination-address": "2NBkFMN1h5Jchqwh4Fp5JYcUEwNuXnU9cR2"
}
```

> Code samples

```shell
curl -request PUT https://atomic.org/api/loans/v1/investors/{user-id}/investments/{investment-id} \
-H 'Content-Type: application/json' --data '{ \
          "coin-type": "BTC",   \
          "amount": 1000000000, \
          "min-interest": 5.4,  \
          "max-loan-duration-in-days": 527, \
          "expiration-date": 1737307138,    \
          "destination-address": "2NBkFMN1h5Jchqwh4Fp5JYcUEwNuXnU9cR2"}'
```

#### Return Codes
Status|Meaning|Description
---|---|---
201 | Created | Investment created/updated
400 | Bad Request | Invalid input
406 | Not Acceptable | Cannot update amount to be less than amount in active loans
409 | Conflict | Investment is up to date


### Get Investment Details

Gets the user's investment information

`GET /api/loans/v1/investors/{user-id}/investments/{investment-id}`

> Code samples

```shell
curl -request GET https://atomic.org/api/loans/v1/investors/{user-id}/investments/{investment-id}
```

> Example responses

> 200 Response

```json
{
    "user-id": "d290f1ee-6c54-4b01-90e6-d701748f0200",
    "investment-summary": { "coin-type": "BTC", "amount": 1000000000},
    "amount-in-active-loans": {"coin-type": "BTC","amount": 420000000},
    "num-active-loans": 1,
    "active-loans": [
        {
        "coin-type": "BTC",
        "amount": 420000000,
        "amount-due": 43500000,
        "interest": 6.9,
        "duration-in-days": 527,
        "expiration-date": 1595697538
        }
    ]
}
```

#### Return Codes
Status|Meaning|Description
---|---|---
200| OK | Investment found
404| Not Found | User/Investment not found


### Delete Investment

Request to delete a user's investment

`DELETE /api/loans/v1/investors/{user-id}/investments/{investment-id}`

> Code samples

```shell
curl -request DELETE https://atomic.org/api/loans/v1/investors/{user-id}/investments/{investment-id}
```

#### Return Codes
Status|Meaning|Description
---|---|---
200| OK | Investment deleted successfully
403| Forbidden | Investment is in progress, unable to delete
404| Not Found |  User/Investment not found


### List Investments

Gets the user's investments information

`GET /api/loans/v1/investors/{user-id}`

> Code samples

```shell
curl -request GET https://atomic.org/api/loans/v1/investors/{user-id}
```

> Example responses

> 200 Response

```json
{
    "user-id": "d290f1ee-6c54-4b01-90e6-d701748f0200",
    "num-investments": 1,
    "investment-info": [
        {
            "investment-summary": { "coin-type": "BTC", "amount": 1000000000},
            "amount-in-active-loans": {"coin-type": "BTC","amount": 420000000},
            "num-active-loans": 1,
            "active-loans": [
                {
                "coin-type": "BTC",
                "amount": 420000000,
                "amount-due": 43500000,
                "interest": 6.9,
                "duration-in-days": 527,
                "expiration-date": 1595697538
                }
            ]
        }
    ]
}
```

#### Return Codes
Status|Meaning|Description
---|---|---
200 | OK | OK
404 | Not Found | User not found

### Get Supported Coins

List of supported coins

`GET /api/loans/v1/coins`

> Code samples

```shell
curl -request GET https://atomic.org/api/loans/v1/coins
```

> Example responses

> 200 Response

```json
{
    ["BTC", "LTC", "DASH", "BSV", "DOGE", "BCH", "ETC", "ETH"] 
}
```

### Get Generic Info

Generic info from the P2P Loans business

`GET /api/loans/v1/info`

> Code samples

```shell
curl -request GET https://atomic.org/api/loans/v1/info
```

> Example responses

> 200 Response

```json
{
    "business-name": "Shark - P2P Lines",
    "business-id": "a418bcfa-abcd-1234-90e6-d701748f0400",
    "api-version": "v1"
}
```

# Business APIs

This section details the APIs from an ATOMIC powered business towards ATOMIC P2P Collateral Guarantees Service

## Deal APIs

Deal management APIs

### Create Deal

Create a new deal

`PUT /api/v1/businesses/{business-id}/deals/{deal-id}`

> Body parameter

```json
{
"num-transactions": 1,
    "transaction-info": [
        {
        "user-id": "d290f1ee-6c54-4b01-90e6-d701748f0200",
        "coin-type": "BTC",
        "target-address": "2NBkFMN1h5Jchqwh4Fp5JYcUEwNuXnU9cR2",
        "escrow-address": "2NBkFMN1h5Jchqwh4Fp5JYcUEwNuXnU9cR2",
        "amount": 854320000,
        "collateral": 854320000,
        "fee": 5123,
        "second-target-address": "2NBkFMN1h5Jchqwh4Fp5JYcUEwNuXnU9cR2",
        "second-amount": 854320000
        }
    ]
}
```

> Code samples

```shell
curl -request PUT https://atomic.org/api/deals/v1/businesses/{business-id}/deals/{deal-id} \
-H 'Content-Type: application/json' \ 
              --data '{ "num-transactions": 1, \
                        "transaction-info": [ \
                            {  \
                            "user-id": "d290f1ee-6c54-4b01-90e6-d701748f0200", \
                            "coin-type": "BTC", \
                            "target-address": "2NBkFMN1h5Jchqwh4Fp5JYcUEwNuXnU9cR2", \
                            "escrow-address": "2NBkFMN1h5Jchqwh4Fp5JYcUEwNuXnU9cR2", \
                            "amount": 854320000, \
                            "collateral": 854320000, \
                            "fee": 5123, \
                            "second-target-address": "2NBkFMN1h5Jchqwh4Fp5JYcUEwNuXnU9cR2", \
                            "second-amount": 854320000 } \
                            ]}'

```

#### Return Codes
Status|Meaning|Description
---|---|---|
201| Created | Deal created
400| Bad Request | Invalid input
409| Conflict | Deal already exists


### Get Deal Details

Gets the deal's information

`GET /api/v1/businesses/{business-id}/deals/{deal-id}`

> Code samples

```shell
curl -request GET https://atomic.org/api/deals/v1/businesses/{business-id}/deals/{deal-id} 
```

> Example responses

> 200 Response

```json
{
    "deal-id": "d290f1ee-6c54-4b01-90e6-d701748f0100",
    "business-id": "d290f1ee-6c54-4b01-90e6-d701748f0400",
    "deal-status": "READY",
    "num-transactions": 2
}
```

#### Return Codes
Status|Meaning|Description
---|---|---
200 | OK | Deal found
400 | Bad Request | Invalid input
404 | Not Found | Business/Deal not found


### Abort Deal

Aborts a deal, if allowed

`DELETE /api/v1/businesses/{business-id}/deals/{deal-id}`

> Code samples

```shell
curl -request DELETE https://atomic.org/api/deals/v1/businesses/{business-id}/deals/{deal-id}
```

#### Return Codes
Status|Meaning|Description
---|---|---
200 | OK | Deal aborted successfully
400 | Bad Request | Invalid input
403 | Forbidden | Deal already in execution phase, abort isn't allowed


### Get Transaction Details

Gets a deal's transaction information

`GET /api/v1/businesses/{business-id}/deals/{deal-id}/transactions/{transaction-index}`

> Code samples

```shell
curl -request GET https://atomic.org/api/deals/v1/businesses/{business-id}/deals/{deal-id}/transactions/{transaction-index}

```

> Example responses

> 200 Response

```json
{
    "user-id": "d290f1ee-6c54-4b01-90e6-d701748f0200",
    "transaction-status": "Pending User Confirmation"
}
```

#### Return Codes
Status|Meaning|Description
---|---|---
200 | OK | OK
400 | Bad Request | Invalid input
404 | Not Found  | Business/Deal/Transaction not found
