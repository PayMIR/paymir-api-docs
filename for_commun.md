# PayMIR API for Commun
# Calculate:

To calculate BTC  

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
Success: | {   result:true,    "inAmount": ``` CMN amount ``` ,   "rate": ``` BTC/CMN rate ``` ,    "outAmount": ``` BTC amount ``` ,   description: }
Error: | {   result: false,   description: ```error message```}

# Exchange:

To exchaange CMN to BTC  

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
Success: | {    "result": true,    "inAmount": "804.0000",    "minAmount": "0.001",    "outAmount": "0.001007722306",    "fees": "0.000507576494",    "rate": "0.0000018847"}
Error: | {   result: false,   description: ```error message```}

