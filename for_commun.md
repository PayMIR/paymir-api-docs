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
Success: | {   result:true,    "inAmount": ``` CMN amont ``` ,   "rate": ``` BTC/CMN rate ``` ,    "outAmount": ``` BTC amount ``` ,   description: }
Error: | {   result: false,   description: ```error message```}
