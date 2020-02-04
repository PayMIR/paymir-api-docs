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
hash | string | md5(implode('', [clientID, currency, amount, secretKey]));

**secretKey** - Partner's secret

**Response:**

Format | JSON
----- | -----
Success: | {   result:true,   url: ```invoice link``` }
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
Success: | {   result:true, data: <br> [ <br> {"type": "deposit", <br> "currency": ``` ISO_4217 Currency code ```, <br> "amount": ``` Amount specified in the invoice ```, <br>             "balance_amount": ``` Actual amount deposited on the bank account ``` , <br> "payment_id": ``` payment number ``` , <br> "time": ``` transaction date ``` , <br> "status": ``` done, pending, canceled ``` , <br>            "clientid": ``` The identifier of your end-user on your platform. ``` }, <br> {...}, <br> {...} <br> ] <br> }
Error: | {   result: false,   description: ```error message```}

# Webhooks

On the API page in your account, enter the callback URL where real-time information on your deposits will be recived. <br> 
_If you want more security also provide us a Secret Key hash of hook signature (if you have a unique signature verification implemented)_

Request type: **POST**

Key | Value
---- | -----
content-type: | application/json
body: | {"type": "deposit", <br>"currency": ISO_4217 Currency code, <br>"amount": Amount specified in the invoice, <br>"balance_amount": Actual amount deposited on the bank account , <br>"payment_id": payment number , <br>"time": transaction date , <br>"status": done, pending, canceled , <br>"clientid": The identifier of your end-user on your platform., <br>"hash": md5(implode('', array( "type", "currency", "amount", "time", "Secret Key hash of hook signature"))))} <br>

# Withdrawal:

To withdraw RUB a user of a **PayMIR** partner sends to **PayMIR** their bank details.

* Access Point https://dev.paymir.io/paymir/api/ajaxwithdrawal/

Request type: **POST**

**Request Data:**

Key | Type | Description
----- | ----- | -----
apiKey | string | Partner’s PayMIR API Key
currency | string | ISO_4217 Currency code
amount | decimal | Invoice amount
BIK | string | BIK of client’s bank
bankAccount | string | The bank account of the client’s bank
cptName | string | Client First Name and Last Name
hash | string | md5(implode('', [currency, amount, BIK, bankAccount, secretKey]));

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

