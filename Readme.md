# International Money Transfer Operator

### Cowrie Transfer Services for remittance
Tihs API enables International Money Transfer Operators to terminate cross border remittance payments to Nigerian mobile-money and bank accounts.

Here are the steps to perform remittance:
* Submit KYC for a sender
* Request a new remittance transfer
* Send the funds via Stellar to complete the funds transfer


## Authentication
The authentication for this API is based [Stellar SEP010](https://github.com/stellar/stellar-protocol/blob/master/ecosystem/sep-0010.md)

The authentication flow follows these steps:
* Get a challenge
* Sign the challenge with your private key
* Send the challenge to retrieve a JWT web token
* Include the JWT in subsequent requests

## Getting the authentication challenge

### Challenge Request
GET https://api.cowrie.exchange/web_auth?account=GCO6BRKD26BDPGGJTXM6XKKKIUN43IDKUNHB7VAYDJKQMU4RVUJ4ITLD

### Challenge Response
HTTP 200 OK
```js
{
    "transaction": "AAAAAgAAAABhlySaNZNO1DPzQrHou0Oq7FAjBfFKb1dhxl5Fg3UjagAAAMgAAAAAAAAAAAAAAAEAAAAAZlhHXQAAAABmWZjdAAAAAAAAAAIAAAABAAAAAJ3gxUPXgjeYyZ3Z66lKRRvNoGqjTh/UGBpVBlORrRPEAAAACgAAABRjb3dyaWUuZXhjaGFuZ2UgYXV0aAAAAAEAAABAdEVycnQ5eElXeVRJdEsxdlZrb2FnUTNCM09VOTgxQk5pdGRGUTJDREZNQXhYL3E4U0FBRVBvWUhlMDZaTGwrbgAAAAEAAAAAYZckmjWTTtQz80Kx6LtDquxQIwXxSm9XYcZeRYN1I2oAAAAKAAAAD3dlYl9hdXRoX2RvbWFpbgAAAAABAAAAD2Nvd3JpZS5leGNoYW5nZQAAAAAAAAAAAYN1I2oAAABA1YlXqfOmb/NHaa56gB4cT37iABD4rXWSsLSUAMASDW3jT4m3y5EqZrgnXTJPsXlWJg+g0vwgqaFZ4a2q6/GBAg==",
    "network_passphrase": "Public Global Stellar Network ; September 2015"
}
```
The trasnaction field in the challenge response is in Stellar XDR format and needs to be signed by the client private key, then sent to the server to retrieve a JWT authentication token

### Token Request
POST https://api.cowrie.exchange/web_auth

Content-Type: application/json
```js
{
    "transaction": "AAAAAgAAAABhlySaNZNO1DPzQrHou0Oq7FAjBfFKb1dhxl5Fg3UjagAAAMgAAAAAAAAAAAAAAAEAAAAAZlhHXQAAAABmWZjdAAAAAAAAAAIAAAABAAAAAJ3gxUPXgjeYyZ3Z66lKRRvNoGqjTh/UGBpVBlORrRPEAAAACgAAABRjb3dyaWUuZXhjaGFuZ2UgYXV0aAAAAAEAAABAdEVycnQ5eElXeVRJdEsxdlZrb2FnUTNCM09VOTgxQk5pdGRGUTJDREZNQXhYL3E4U0FBRVBvWUhlMDZaTGwrbgAAAAEAAAAAYZckmjWTTtQz80Kx6LtDquxQIwXxSm9XYcZeRYN1I2oAAAAKAAAAD3dlYl9hdXRoX2RvbWFpbgAAAAABAAAAD2Nvd3JpZS5leGNoYW5nZQAAAAAAAAAAAoN1I2oAAABA1YlXqfOmb/NHaa56gB4cT37iABD4rXWSsLSUAMASDW3jT4m3y5EqZrgnXTJPsXlWJg+g0vwgqaFZ4a2q6/GBApGtE8QAAABAPYcs7uceRXCG5KZeMER2K7+2muqs2nbODR0qQsol6LgbJNQRRJsF189N4DUuJCsG7FZnH8JFxLz+4vFXWfKWDw=="
}
```

### Token Response
HTTP 200 OK
```js
{
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJodHRwczovL2FwaS5jb3dyaWUuZXhjaGFuZ2UiLCJzdWIiOiJHQ082QlJLRDI2QkRQR0dKVFhNNlhLS0tJVU40M0lES1VOSEI3VkFZREpLUU1VNFJWVUo0SVRMRCIsImlhdCI6MTcxNzA2MTQ2OSwiZXhwIjoxNzE3MTQ3ODY5LCJqdGkiOiI5NTkyNmE1OGQxYWYzMmIxZGNhNzc5NmNmNjI2NWQwODRjNDJjMGQ3Y2FhNDU2MjA3NjIzZmM5MzE3N2Y3NjE4In0.mLrOO8rJ5svHyrc9jI5os2Shvsml-jBQH5mqWgaj5yk"
}
```
The token field value is the JWT authentication token that must be included in the Authorization HTTP header for all subsequent requests

---

## Submitting KYC
KYC handling is based on [Stelar SEP012](https://github.com/stellar/stellar-protocol/blob/master/ecosystem/sep-0012.md)
KYC documents are uploaded via endpoint https://api.cowrie.exchange/kyc/customer which also lists required fields

### KYC Upload Request
POST https://api.cowrie.exchange/kyc/customer
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJodHRwczovL2FwaS5jb3dyaWUuZXhjaGFuZ2UiLCJzdWIiOiJHQ082QlJLRDI2QkRQR0dKVFhNNlhLS0tJVU40M0lES1VOSEI3VkFZREpLUU1VNFJWVUo0SVRMRCIsImlhdCI6MTcxNzA2MTQ2OSwiZXhwIjoxNzE3MTQ3ODY5LCJqdGkiOiI5NTkyNmE1OGQxYWYzMmIxZGNhNzc5NmNmNjI2NWQwODRjNDJjMGQ3Y2FhNDU2MjA3NjIzZmM5MzE3N2Y3NjE4In0.mLrOO8rJ5svHyrc9jI5os2Shvsml-jBQH5mqWgaj5yk

Content-Type: multipart/form-data;boundary="boundary"

--boundary
Content-Disposition: form-data; name="account"

GCO6BRKD26BDPGGJTXM6XKKKIUN43IDKUNHB7VAYDJKQMU4RVUJ4ITLD
--boundary
Content-Disposition: form-data; name="first_name"

JENNY
--boundary
Content-Disposition: form-data; name="last_name"

CASH
--boundary
Content-Disposition: form-data; name="id_number"

B0123456789
```

### KYC Upload Response
HTTP 201 Accepted
```js
{
  "id": "36a61a1f79331432bb7de6ef788cdc4cc"
}
```
The id filed in the response represent a sending customer provisioned for remittance

---

## Requesting a Remittance Transfer
Remittance ransfer requests are based on [Stellar SEP031](https://github.com/stellar/stellar-protocol/blob/master/ecosystem/sep-0031.md)
A remittance transfer request is initiated by calling the endpoint https://api.cowrie.exchange/sep31/direct/transactions and specifying the receiver bank account information.

### Remittance Request
POST https://api.cowrie.exchange/sep31/direct/transactions
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJodHRwczovL2FwaS5jb3dyaWUuZXhjaGFuZ2UiLCJzdWIiOiJHQ082QlJLRDI2QkRQR0dKVFhNNlhLS0tJVU40M0lES1VOSEI3VkFZREpLUU1VNFJWVUo0SVRMRCIsImlhdCI6MTcxNzA2MTQ2OSwiZXhwIjoxNzE3MTQ3ODY5LCJqdGkiOiI5NTkyNmE1OGQxYWYzMmIxZGNhNzc5NmNmNjI2NWQwODRjNDJjMGQ3Y2FhNDU2MjA3NjIzZmM5MzE3N2Y3NjE4In0.mLrOO8rJ5svHyrc9jI5os2Shvsml-jBQH5mqWgaj5yk

Content-Type: application/json
```js
{
    "stellar_account": "GCO6BRKD26BDPGGJTXM6XKKKIUN43IDKUNHB7VAYDJKQMU4RVUJ4ITLD",
    "asset_code": "NGNT",
    "asset_issuer": "GAWODAROMJ33V5YDFY3NPYTHVYQG7MJXVJ2ND3AOGIHYRWINES6ACCPD",
    "amount": "1000",
    "sender_id": "36a61a1f79331432bb7de6ef788cdc4cc",
    "fields":
    {
        "transaction":
        {
            "bank_name": "GTBank",
            "account_number": "0005538936",
            "type": "NIBSS"
        }
    }
}
```

### Remittance Response
HTTP 200 OK
```js
{
    "id": "45c681daf258451b963cbc9ae5a3d209",
    "stellar_account_id": "GBQZOJE2GWJU5VBT6NBLD2F3IOVOYUBDAXYUU32XMHDF4RMDOURWV3GT",
    "stellar_memo_type": "hash",
    "stellar_memo": "d3f43208a1d048c6a0da263aa2a223346f9ece0d33304fcd9a4967a008971cca",
    "receiver": {
        "account_name": "JOHNNY CASH"
    }
}
```
The id field in the response uniquely identifies this transaction and can be used to query the transation status.
The receiver field in the response contains the account_name key whose value is the validated bank account name corresponding tot tthe bank account number

---

## Sending funds via Stellar
To complete the funds transfer, make a Stellar payment using these parameters
stellar 
Parameter|Value
---|---
account|GBQZOJE2GWJU5VBT6NBLD2F3IOVOYUBDAXYUU32XMHDF4RMDOURWV3GT
memo type|hash
memo|d3f43208a1d048c6a0da263aa2a223346f9ece0d33304fcd9a4967a008971cca
asset|NGNT
asset issuer|GAWODAROMJ33V5YDFY3NPYTHVYQG7MJXVJ2ND3AOGIHYRWINES6ACCPD
amoun|1000


## Checking the transaction status
TO check the status of a funds transfer, call the endpoint https://api.cowrie.exchange/sep31/direct/transactions/45c681daf258451b963cbc9ae5a3d209

### Transaction Status Request
GET  https://api.cowrie.exchange/sep31/direct/transactions/45c681daf258451b963cbc9ae5a3d209
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJodHRwczovL2FwaS5jb3dyaWUuZXhjaGFuZ2UiLCJzdWIiOiJHQ082QlJLRDI2QkRQR0dKVFhNNlhLS0tJVU40M0lES1VOSEI3VkFZREpLUU1VNFJWVUo0SVRMRCIsImlhdCI6MTcxNzA2MTQ2OSwiZXhwIjoxNzE3MTQ3ODY5LCJqdGkiOiI5NTkyNmE1OGQxYWYzMmIxZGNhNzc5NmNmNjI2NWQwODRjNDJjMGQ3Y2FhNDU2MjA3NjIzZmM5MzE3N2Y3NjE4In0.mLrOO8rJ5svHyrc9jI5os2Shvsml-jBQH5mqWgaj5yk
```

### Transaction Status Response
HTTP 200 OK
```js
{
    "transaction": {
        "id": "45c681daf258451b963cbc9ae5a3d209",
        "status": "completed",
        "stellar_account_id": "GCFPTZR3EFKY5XNXYH7IIWVMXXKRYUXFZNKSBPB433RCTN7Y2K4BRYQG",
        "stellar_memo_type": "hash",
        "stellar_memo": "7d4ffdfef6fb4f87adb1ca1d2a2a6358c7635241b2d84e56a5aa7ecab0e41cbb",
        "amount_in": "1000.0000000",
        "amount_out": "800",
        "amount_fee": "200"
    }
}
```
