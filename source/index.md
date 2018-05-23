---
title: API Reference

language_tabs:
  - bash

includes:
  - errors

search: true
---

# Introduction

Welcome to the DVC OTC API. You can use our API to access DVC OTC API endpoints, that expose data such as trades, prices, and user information, depending on your scope.


# Authentication

> To retrieve your JWT, use this code:

```bash
curl "http://dv-otc-prod.azurewebsites.net/api/v3/auth"
  -H "Authorization: ZGVtbzpwQDU1dzByZA=="
```

This API uses [JSON Web Tokens](https://jwt.io/) (JWTs) to allow access to the API. You can retrive a new token using your DVC OTC credentials through [BASIC authentication](https://swagger.io/docs/specification/authentication/basic-authentication/).

The API expects for your JWT to be included in all API requests to the server in a header that looks like the following:

`Authorization: bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c`

<aside class="notice">
You must replace <code>eyJhbGciOiJ...</code> with your JWT.
</aside>

# Trades

## Get All Trades

```bash
curl "http://dv-otc-prod.azurewebsites.net/api/v3/trades"
  -H "Authorization: bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
```

> The above command returns JSON structured like this:

```json
{
    "data": [
        {
            "_id": "5b04840d610af6110456057f",
            "updatedAt": "2018-05-22T20:56:45.466Z",
            "createdAt": "2018-05-22T20:56:45.466Z",
            "price": 1147.77,
            "indexPrice": 1147.77,
            "quantity": 2,
            "value": 2295.54,
            "side": "Sell",
            "user": {
                "_id": "5ac67efc22e7222304f49f42",
                "updatedAt": "2018-05-22T20:56:45.528Z",
                "createdAt": "2018-04-05T19:54:36.359Z",
                "email": "roger@bitcoin.cash",
                "name": "Roger Ver",
                "tokens": [],
                "admin": false,
                "__v": 0,
                "internal": false,
                "active": true,
                "firstName": "Roger",
                "groupAccount": "1",
                "lastName": "Ver",
                "profile": {
                    "gender": "",
                    "location": "",
                    "website": ""
                },
                "cashBalance": 2296.54
            },
            "asset": "BCH",
            "status": "Active",
            "settlement": {
                "assetReceived": false,
                "assetSent": false,
                "fiatReceived": false,
                "fiatSent": false
            },
            "__v": 0
        }
    ]
}
```

This endpoint retrieves all trades.

### HTTP Request

`GET http://dv-otc-prod.azurewebsites.net/api/v3/trades`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
before | none | If set, will only return trades before this date/time.
after | none |  If set, will only return trades after this date/time.

# Trade Parameters

## Update Balance

```bash
curl "http://dv-otc-prod.azurewebsites.net/api/v3/updateBalance"
  -H "Authorization: bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
  -d '{"balance": 1000, "groupAccount": 1, "asset": "USD"}'
```

> The above command returns JSON structured like this:

```json
{
    "asset": "USD",
    "updatedBalance": 1000,
    "groupAccount": "1"
}
```

This endpoint updates a user's balance.

### HTTP Request

`POST http://dv-otc-prod.azurewebsites.net/api/v3/updateBalance`

### Body Parameters

Parameter | Required? | Description
--------- | ------- | -----------
groupAccount | true | The group account number of the user whose balance should be updated.
asset | true | The asset that is being updated.
balance | true | The balance value that should be set.