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

