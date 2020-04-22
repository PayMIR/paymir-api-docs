# PayMIR API for RUB
# Deposit:

To deposit RUB through **PayMIR** a user of a **PayMIR** partner receives an invoice with their unique ID (cliendID) and pays in their bank.

* Access Point: https://dev.paymir.io/paymir/api/ajaxinvoicing/

Request type: **POST**

**Request Data:**

Key | Type | Description
----- | ----- | -----
apiKey | string | Partner's PayMIR API Key
clientID | string | The identifier of your end-user on your platform (partner's platform).
currency | string | ISO_4217 Currency code
cptName | string | Client First Name and Last Name
amount | decimal | Invoice amount
additionData[address] | string |  ``` USDT address of end-user ```
hash | string | md5(implode('', [clientID, currency, amount, secretKey]));


**secretKey** - Partner`s secret

**Response:**

Format | JSON
----- | -----
Success: | {   result:true, <br> url: ```invoice link```, <br> invoiceData: Beneficiary Info to make invoice <br> (```Name: ... Acc RUB: ... Beneficiary Address: ... Bank:... SWIFT: .... Payment details: ...```) <br>}
Error: | {   result: false,   description: ```error message```}

# Paymir USDT/RUB rate

To get Paymir USDT/RUB rate and fee

* Access Point: http://dev.paymir.io/paymir/api/ajaxkucoinrates/

Request type: **POST**

Key | Type | Description
----- | ----- | -----
apiKey | string | Partner's PayMIR API Key
amount | string | RUB amount
hash | string | md5(implode('', [amount, secretKey]));

**secretKey** - Partner`s secret

**Response:**

Format | JSON
----- | -----
Success: | {"result": true, <br> "usdtRate": ``` Paymir USDT/RUB rate ```, <br> "feePercent": ``` Paymir fee ```, <br> "calculatedUsdtAmount": calculated USDT amount}
Error: | {   result: false,   description: ```error message```}

# TxID

To get RUB deposit exchanged to USDT (deposit "status" = ``` paid ``` ) send us ERC20 Tether TxID of end-user account replenishment. <br>
_Attention: Deposit will be bloked until the TxId is sent._

* Access Point: https://dev.paymir.io/paymir/api/ajaxtxreporting

Request type: **POST**

Key | Type | Description
----- | ----- | -----
apiKey | string | Partner's PayMIR API Key
paymentId | string | ``` paymentId ``` from webhook
address | string | ``` USDT address of end-user ```
txId| string |  ERC20 Tether TxID
hash | string | md5(implode('', [paymentId,address,txId, secretKey]));

**secretKey** - Partner`s secret

**Response:**

Format | JSON
----- | -----
Success: | {   result:true,   totalAmount: ```USDT Amount ``` }
Error: | {   result: false,   description: ```error message```}

# Deposits history

* Access Point: https://dev.paymir.io/paymir/api/ajaxdeposits/

Request type: **GET**

**Request Data:**

Key | Type | Required | Description
----- | ----- | ----- | -----
apiKey | string | Yes | Partner’s PayMIR Api Key
limit | decimal | No | Numbers per page <br> ``` min:5, max:100, default:5```
page | decimal | No | Page number <br> ```default: 0```
hash | string | Yes | md5(implode('', [limit, page, secretKey]));

**secretKey** - Partner`s secret

**Response:**

Format | JSON
----- | -----
Success: | {   result:true, data: <br> [ <br> {"type": "deposit", <br> "currency": ``` ISO_4217 Currency code ```, <br> "amount": ``` Amount specified in the invoice ```, <br>             "balance_amount": ``` Actual amount deposited on the bank account ``` , <br> "payment_id": ``` payment number ``` , <br> "time": ``` transaction date ``` , <br> "status": ``` done, pending, paid, canceled ``` , <br>            "clientid": ``` The identifier of your end-user on your platform. ``` }, <br> {...}, <br> {...} <br> ] <br> }
Error: | {   result: false,   description: ```error message```}

# Webhooks

On the API page in your account, enter the callback URL where real-time information on your deposits will be recived. <br> 
_If you want more security also provide us a Secret Key hash of hook signature (if you have a unique signature verification implemented)_

Request type: **POST**

Key | Value
---- | -----
content-type: | application/json
body: | {"type": "deposit", <br>"currency": ISO_4217 Currency code, <br>"amount": Amount specified in the invoice, <br>"balance_amount": Actual amount deposited on the bank account , <br>"payment_id": payment number , <br>"time": transaction date , <br>"status": done, paid, pending, canceled , <br>"clientid": The identifier of your end-user on your platform., <br> ``` "tx_address": USDT address of end-user, ```  <br> ``` "usdt_amount": calculate USDT balance Amount, ``` <br> ``` (For "status": paid) ``` <br> "hash": md5(implode('', array( "type", "currency", "amount", "time", "Secret Key hash of hook signature"))))} <br>


# Withdrawal:

An available method should be determined to use withdrawals. As a result you will get a list of available methods of withdrawal.

* Access Point https://dev.paymir.io/paymir/api/ajaxpayoutprerequest/

Request type: **GET**

**Request Data:**

Key | Type | Description
----- | ----- | -----
apiKey | string | Partner’s PayMIR API Key
currency | string | ISO_4217 Currency code
hash | string | md5(implode('', [currency, secretKey]));

Responce example with list of withdraw methods

```
 {
    "result": true,
    "methods": [
        {
            "name": "fiat_wire_lp_3",
            "required_fields": [
                "apiKey",
                "currency",
                "amount",
                "method",
                "cptName",
                "BIK",
                "bankAccount",
                "hash"
            ],
            "min_usd_fee": "0.00",
            "fixed_usd_fee": "100.000000000000",
            "percent_fee": "4.10",
            "min_amount_usd": "5000.0000",
            "anti_fraud_percent": "30.00"
        },
        {
            "name": "fiat_card_lp_1",
            "required_fields": [
                "apiKey",
                "currency",
                "amount",
                "method",
                "cptName",
                "cardNumber",
                "hash"
            ],
            "min_usd_fee": "0.00",
            "fixed_usd_fee": "100.000000000000",
            "percent_fee": "25.00",
            "min_amount_usd": "0.0000",
            "anti_fraud_percent": "30.00"
        }
    ]
} 
```

You must select avalible withdraw method.

To withdraw funds a user of a **PayMIR** partner sends to **PayMIR** their bank or crypto details

* Access Point https://dev.paymir.io/paymir/api/ajaxwithdrawal/

Request type: **POST**

**Request Data:**

Key | Type | Description
----- | ----- | -----
apiKey | string | Partner’s PayMIR API Key
currency | string | ISO_4217 Currency code
amount | decimal | Invoice amount
method | string | Method ``` name ```  from Pre-request response
BIK | string | BIK of client’s bank
bankAccount | string | The bank account of the client’s bank
cptName | string | Client First Name and Last Name
cryptoAddress | string | Your wallet Address (for cryptocurrency)
cardNumber | string | Number of your Visa/MasterCard Bank card
hash | string | md5(implode('', [currency, amount, method, BIK, bankAccount, cryptoAddress, cardNumber, secretKey]));

**secretKey** - Partner's secret

**Response:**

Format | JSON
----- | -----
Success: | {   result:true,   document: ```withdrawalID```}
Error: | {   result: false,   description: ```error message```}


## Check withdrawal:
* Access Point https://dev.paymir.io/paymir/api/ajaxwithdrawal/

Request type: **GET**

**Request Data:**

Key | Type | Description
----- | ----- | -----
apiKey | string | Partner’s PayMIR Api Key
withdrawalID | string | Document ID
hash | string | md5(implode('', [withdrawalID, secretKey]));

**secretKey** - Partner's secret

**Response:**

Format | JSON
----- | -----
Success: | {   result:true,   document: ```withdrawalID```,   status: ```document status```}
Error: | {   result: false,   description: ```error message```}

