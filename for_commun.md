# PayMIR API for Commun
# Calculate:

To get preliminary BTC amount

* Access Point: https://dev.paymir.io/paymir/api/ajaxcmncalculate

Request type: **POST**

**Request Data:**

Key | Type | Description
----- | ----- | -----
apiKey | string | Partner's PayMIR API Key
amount | decimal | CMN amount
hash | string | md5(implode('', [amount, secretKey]));


**secretKey** - Partner`s secret

**Response:**

Format | JSON
----- | -----
Success: | {   result:true,    "inAmount": ``` CMN amount ``` , "minAmount": ''' min limit BTC amount ``` ,  "rate": ``` BTC/CMN rate ``` ,    "outAmount": ``` BTC amount ``` ,   description: }
Error: | {   result: false,   description: ```error message```}

# Exchange:

To exchange CMN to BTC  

* Access Point: https://dev.paymir.io/paymir/api/ajaxcmntxreporting

Request type: **POST**

**Request Data:**

Key | Type | Description
----- | ----- | -----
apiKey | string | Partner's PayMIR API Key
txId | decimal | CMN amount
hash | string | md5(implode('', [txId, secretKey]));


**secretKey** - Partner`s secret

**Response:**

Format | JSON
----- | -----
Success: | {    "result": true,    "inAmount": ``` CMN amount ```,    "minAmount": ``` min limit BTC amount ```,    "outAmount": ``` BTC amount ```,    "fees": ``` total excange fee ``` ,    "rate": ``` BTC/CMN rate ```}
Error: | {   result: false,   description: ```error message```}

# Webhooks

Provide your callback URL on the API page of the PayMIR personal area. The URL will be received successful transaction. <br> 
_In case you need additional security, provide us with your Secret Key hash of the hook signature (if you have a unique signature verification implemented)._

Request type: **POST**

Key | Value
---- | -----
content-type: | application/json
body: | {"type":"withdrawal", <br> "currency":"BTC" (ISO_4217 Currency code), <br> "amount": technical BTC amount, <br> "balance_amount":Actual BTC amount of exchange, <br> "payment_id":payment number, <br> "time":transaction date, <br> "status":"done", <br> "balance_currency":"BTC", <br> "clientid":null, <br> "CMNtx":id of CMN transaction, <br> "BTCtx": id of BTC transaction , <br> "hash":"hash": md5(implode('', array( "type", "currency", "amount", "time", "Secret Key hash of hook signature"))))}



