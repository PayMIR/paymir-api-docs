# PayMIR API for RUB
# Deposit:

To deposit RUB through **PayMIR** a user of a **PayMIR** partner receives an invoice with their unique ID (cliendID) and pays in their bank.

* Access Point: https://dev.paymir.io/paymir/api/ajaxinvoicing/

Request type: **POST**

**Request Data:**

Key | Type | Description
----- | ----- | -----
apiKey | string | Partner’s PayMIR API Key
clientID | string | The identifier of your end-user on your platform (partner's platform).
currency | string | ISO_4217 Currency code
amount | decimal | Invoice amount
hash | string | md5(implode('', [clientID, currency, amount, secretKey]));

**secretKey** - Partnerr`s secret

**Response:**

Format | JSON
----- | -----
Success: | {   result:true,   url: ```invoice link``` }
Error: | {   result: false,   description: ```error message```}

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

**secretKey** - Partner’s secret

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

**secretKey** - Partner’s secret

**Response:**

Format | JSON
----- | -----
Success: | {   result:true,   document: ```withdrawalID```,   status: ```document status```}
Error: | {   result: false,   description: ```error message```}

