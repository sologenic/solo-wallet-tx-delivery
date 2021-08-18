# Solo Wallet Transaction Delivery API

## Root URL
`https://api.sologenic.org`

### Initiate Transaction
`POST /api/v1/issuer/transactions`

#### Body
- `tx_json` *required* - transaction template in JSON format as per XRPL transactions specifications (exception pseudo 'SignIn' type). The next fields which will be identified during signing can be omitted: Sequence, LastLedgerSequence.
- `options` *optional* - JSON with optional parameters
- `options.identifier` *optional* - unique UUID v4 transaction identifier 
- `options.submit` *optional, default true* - identifies if mobile application have to submit signed transaction to XRP ledger
- `options.expires_at` *optional* - transaction expiration datetime in RFC3339 format
- `options.push_token` *optional* - push token to deliver a transaction sign request directly to the mobile device of a user via notification

#### Response Example
```json5
{
  "meta": {
    "identifier": "8b528257-eacd-4ab9-85c5-ed10d107f2db",
    "expires_at": "2021-05-14T18:14:32Z",
    "submit": false,
    "pushed": false,
    "opened": false,
    "resolved": false,
    "signed": false,
    "cancelled": false,
    "expired": false
  },
  "refs": {
    "qr": "https://qr.test.sologenic.org?data=https%3A%2F%2Ftest.sologenic.org%2Ftransactions%2F9e6e7c38-4a25-40eb-8cdb-e94e84ec4889",
    "ws": "ws://127.0.0.1:8081/ws/v1/issuer/transactions/9e6e7c38-4a25-40eb-8cdb-e94e84ec4889",
    "deeplink": "https://test.sologenic.org/transactions/9e6e7c38-4a25-40eb-8cdb-e94e84ec4889"
  },
  "tx_json": {
    "TransactionType": "Payment",
    "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
    "Amount": {
      "currency": "USD",
      "value": "100000",
      "issuer": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn"
    }
  }
}
```

#### Request Example

##### Without Options
```bash
curl --location --request POST 'https://api.test.sologenic.org/api/v1/issuer/transactions' \
--header 'Content-Type: application/json' \
--data-raw '{
    "tx_json": {
      "TransactionType" : "Payment",
      "Account" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "Destination" : "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
      "Amount" : {
         "currency" : "USD",
         "value" : "1",
         "issuer" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn"
      }
    }
}'
```

##### With Options
```bash
curl --location --request POST 'https://api.test.sologenic.org/api/v1/issuer/transactions' \
--header 'Content-Type: application/json' \
--header 'Cookie: __cfduid=d5cad91d8911f1f2a0a52d49d0912be161613652122' \
--data-raw '{
    "tx_json": {
      "TransactionType" : "Payment",
      "Account" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "Destination" : "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
      "Amount" : {
         "currency" : "USD",
         "value" : "1",
         "issuer" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn"
      }
    },
    "options": {
        "identifier": "09eba801-9f00-4b52-ad69-bac0027a54ad",
        "submit": false,
        "push_token": "eyJhbGciOiJFUzUxMiIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJzZ190eF9kZWxpdmVyeSIsImV4cCI6MTYzMTM2Mjc1NiwianRpIjoiZGE2YzMzYjItZGE4ZS00NmJkLThiOWYtYmU4YWRhNDM4Y2NmIiwiaWF0IjoxNjI4NzcwNzU2LCJhZGRyZXNzIjoickVhV3BVR0NURThuRG1MUjJERVF6a1IxRXJmcmYxalJDdSJ9.AR0TGiliEWg3bUljStLphJZS2j-vp8zJLO7aWEWTXyCYQt3MYZSDx6zp70GCf8FXR0nAPrhRHCNz-7wWjNlYFfgjAfLhUmxteLSXTsgjfG6JUMx-wq-w1_miSkmNSCU41xZqsWfNys8xIDABNBbhf-npJt0CkDK-rpIzFwh3HmVY961_",
        "expires_at": "2022-02-18T18:20:57Z"
    }
}'
```

### Read Transaction
`GET /api/v1/issuer/transactions/:identifier`

#### Params
- `identifier` *required* - unique UUID v4 transaction identifier

#### Response

##### Opened & Cancelled Transaction
```json5
{
    "meta": {
        "identifier": "09eba801-9f00-4b52-ad69-bac0027a54ad",
        "expires_at": "2022-02-18T18:20:57Z",
        "submit": false,
        "pushed": false,
        "opened": true,
        "resolved": true,
        "signed": false,
        "cancelled": true,
        "expired": false
    },
    "tx_json": {
        "Amount": {
            "value": "1",
            "issuer": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
            "currency": "USD"
        },
        "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
        "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
        "TransactionType": "Payment"
    }
}
```
##### Opened & Signed Transaction
```json5
{
    "meta": {
        "identifier": "ca601a91-c86d-4a7f-a0d9-e034b3891ca9",
        "expires_at": "2021-02-18T20:29:43Z",
        "submit": false,
        "pushed": false,
        "opened": false,
        "resolved": true,
        "signed": true,
        "cancelled": false,
        "expired": false
    },
    "tx_json": {
        "Amount": {
            "value": "1",
            "issuer": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
            "currency": "USD"
        },
        "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
        "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
        "TransactionType": "Payment"
    },
    "signer": "rEaWpUGCTE8nDmLR2DEQzkR1Erfrf1jRCu",
    "txid": "EEF3414E7D7BEE180046A3AA44F3B289FBA6B48BC95FCC1D9AB8AC1D378CB919",
    "tx_hex": "12000722800000002400DECCA12A298E785F201B00DF312F6440000000000F424065D4038D7EA4C680000000000000000000000000005553440000000000853D6FBDC845D9C1E83002A67E8E8DD6CE6995B168400000000000000C81146F22BDBF76F75F948A4E9CC39DBE482A65C17DDD"
}
```

#### Example
```bash
curl --location --request GET 'https://api.test.sologenic.org/api/v1/issuer/transactions/ca601a91-c86d-4a7f-a0d9-e034b3891ca9'
```
