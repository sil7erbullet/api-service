# thecoingate.com API integration

## Environments
| Name      | API Endpoint | 
| ----------- | ----------- |
| production      | https://api.thecoingate.com/v1/public-api|
| staging      | https://api-staging.thecoingate.com/v1/public-api|



## How to generate HMAC in the request header

The HMAC would be generated by the request body and your private key in SHA512


### Example: Get supported coins

Headers

>  HMAC: c03901975d6dd4cb480ab94453a62a35bd4b44771dfb95a6ce5da789bde3f8391e823281941a7f035c9aa16f07631ca5a55cba64a48ce4db5475234a6b15a37d
>
> Content-Type: application/x-www-form-urlencoded
>
 Payload:

>`version=1.0&key=dc8b7b4b23cf4151e2dc00ff014ca2be5831447f`


## Api integration
### 1. Supported coins
URI: `{{uri}}/supported-coins`

Headers

>  HMAC: `YOUR HMAC`
>
> Content-Type: application/x-www-form-urlencoded
>
Payload:

| Field name      | Description | Require |
| -------- | ----------- |----------- |
| version | The API version | true
| key | Your api public key | true



Reponse: 


```json 
[
  {
    "shortCode": "string",
    "fullCode": "string",
    "assetToken": "string",
    "logo": "string"
  }
]
```

### 2. Balance
URI: `{{uri}}/balance`

Headers

>  HMAC: `YOUR HMAC`
>
> Content-Type: application/x-www-form-urlencoded
>
 Payload:


| Field name      | Description | Require |
| -------- | ----------- |----------- |
| version | The API version | true
| key | Your API public key | true


Reponse: 


```json 
[
  {
    "balance": 0,
    "balancef": "string",
    "code": "string",
    "tokenType": "string",
    "assetBalances": [
      {
        "assetToken": "string",
        "balance": "string",
        "code": "string",
        "tokenType": "string"
      }
    ]
  }
]
```

### 3. Get deposit address
URI: `{{uri}}/deposit-address`

Headers

>  HMAC: `YOUR HMAC`
>
> Content-Type: application/x-www-form-urlencoded
>
 Payload:


| Field name      | Description | Require |
| -------- | ----------- |----------- |
| version | The API version | yes
| key | Your API public key | yes
| identifier | This is unique key helps you to save the deposit address. To avoid re-generating a new deposit address, we recommend that you provide us your user's identifier. Then you have got 1 user = 1 deposit address  | yes
| ipnUrl | The callback URL that you want to receive the instant payment notifications | no
|label | The label for the address | no



Reponse: 


```json 
{
    "address": "string", // Hex in base58 format
    "hexAddress": "string", // Hex address
    "identifer": "string" // User's indentifier
}
```

### 4. Withdrawal
URI: `{{uri}}/withdraw`

Headers

>  HMAC: `YOUR HMAC`
>
> Content-Type: application/x-www-form-urlencoded
>
 Payload:


| Field name      | Description | Require |
| -------- | ----------- |----------- |
| version | The API version | yes
| key | Your API public key | yes
|address | The base 58 address that you want to withdraw to | yes
|currency | The currency that you want to withdraw `TRX, USDT.TRC20 ... `| yes
|tokenType | The type of token. Currently, there are 2 kinds of type: `TRC10, TRC20` | yes
|amount | The amount that you want to withdraw | yes
|autoComfirm | Default is -1. When you withdraw, the system will you a confirmation email to your merchant's email address. You must confirm the transaction before withdrawing. Unless it will be confirmed automatically | no
| ipnUrl | The callback URL that you want to receive the instant payment notifications | no
|label | The label for the address | no




Response: `204 status with no content`
