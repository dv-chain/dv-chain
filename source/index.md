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

# Environment URLs

The DV OTC API is available in both Production as well as Sandbox Mode. Please find the corresponding URLs below

### Production
https://otc.dvchain.co/api/v4

### Sandbox
https://sandbox.otc.dvchain.co/api/v4



# Authentication

> To retrieve your JWT, use this code:

```bash
curl "https://sandbox.otc.dvchain.co/api/v4/auth"
  -H "Authorization: ZGVtbzpwQDU1dzByZA=="
```

This API uses [JSON Web Tokens](https://jwt.io/) (JWTs) to allow access to the API. You can retrive a new token using your DVC OTC credentials through [BASIC authentication](https://swagger.io/docs/specification/authentication/basic-authentication/).
You can request a read only JWT that will only allow actions that will not modify, create, or delete objects.
To retrieve a read only token, include `scope=readOnly` in your query params. Your request URL will look like:

`https://sandbox.trade.dvchain.co/api/v4/auth?scope=readOnly`

The API expects for your JWT to be included in all API requests to the server in a header that looks like the following:

`Authorization: bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c`

<aside class="notice">
You must replace <code>eyJhbGciOiJ...</code> with your JWT.
</aside>

# Prices

## Get Current Asset Prices

```bash
curl "https://sandbox.otc.dvchain.co/api/v4/prices"
  -H "Authorization: bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
```

> The above command returns JSON structured like this:

```json
{
    "BTC": {
        "levels": [
            {
                "sellPrice": 6181.4,
                "buyPrice": 6218.61,
                "maxQuantity": 1
            },
            {
                "sellPrice": 6171.43,
                "buyPrice": 6228.63,
                "maxQuantity": 5
            },
            {
                "sellPrice": 6161.46,
                "buyPrice": 6238.66,
                "maxQuantity": 10
            }
        ],
        "expiresAt": 1539626145342
    },
    "ETH": {
        "levels": [
            {
                "sellPrice": 198.4,
                "buyPrice": 200.6,
                "maxQuantity": 10
            },
            {
                "sellPrice": 197.41,
                "buyPrice": 201.6,
                "maxQuantity": 20
            },
            {
                "sellPrice": 196.41,
                "buyPrice": 202.61,
                "maxQuantity": 30
            }
        ],
        "expiresAt": 1539626145342
    }
}
```

This endpoint retrieves all available products and their prices. 

The levels array includes the asset price by quantity.

For example, an order of up to 1 BTC would be priced at $6,218.61 per Bitcoin for a buy order, and $6,181.40 per Bitcoin for a sell order. An order of up to 5 BTC would be priced at $6,228.63 per Bitcoin for a buy order, and $6,171.43 per Bitcoin for a sell order.

The given price will expire after the "expiresAt" time has passed.

### HTTP Request

`GET https://sandbox.otc.dvchain.co/api/v4/prices`

# RFQ

## Get Current Asset Price By Size

```bash
curl "https://sandbox.otc.dvchain.co/api/v4/RFQ?side=Buy&qty=1&asset=BTC"
  -H "Authorization: bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
```

> The above command returns JSON structured like this:

```json
{
   "asset": "ETH",
   "price": 201.6,
   "qty": 19,
   "side": "Buy",
   "expiresAt": 1540246595317
}
```

This endpoint retrieves the buy/sell price for an asset at the requested quantity.
If you use the `fiat` query parameter instead of `qty`, the endpoint retrieves
the buy/sell quantity at that requested price

The given price will expire after the "expiresAt" time has passed.

### HTTP Request

`GET https://sandbox.otc.dvchain.co/api/v4/RFQ?side=Buy&qty=1&asset=BTC`

### Query Params

Parameter  | Description
--------- | -----------
side | Buy or Sell
qty | The quantity you would like to purchase.
fiat | The amount you would like to spend in USD to purchase.
asset | The asset you would like to purchase.

<aside class="notice">
You must specify `qty` or `fiat`, but not both. Using both query parameters will result in undefined behavior.
</aside>

# Trade

## Execute a Trade

```bash
curl "https://sandbox.otc.dvchain.co/api/v4/trade" \
  -X POST \
  -d '{"side": "Buy","qty": 0.1,"price": 527.51,"asset": "BCH"}' \
  -H "Content-Type: application/json" \
  -H "Authorization: bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
```

> The above command returns JSON structured like this:

```json
{
    "_id": "5bbe696c5f7cec5b98ca5e9a",
    "createdAt": "2018-10-10T21:04:44.361Z",
    "price": 527.51,
    "quantity": 0.1,
    "side": "Buy",
    "user": {
        "_id": "5ab545a4b933aa1f78e25f34",
        "firstName": "Roger",
        "lastName": "Ver"
    },
    "asset": "BTC",
    "status": "Complete"
}
```

This endpoint executes a trade at the current asset price for the given quantity.

### HTTP Request

`POST https://sandbox.otc.dvchain.co/api/v4/trades`

### Request Body

Parameter | Default | Description
--------- | ------- | -----------
orderType | Market | Market or Limit
side | none | Buy or Sell
qty | none | The quantity you would like to purchase.
asset | none | The asset you would like to purchase.
price | none | (Required for a market order) The current price of the asset returned from the /prices or /rfq endpoint.
limitPrice | none | (Required for a limit order) The limit price that you would like to pay.


# Trades

## Get All Trades

```bash
curl "https://sandbox.otc.dvchain.co/api/v4/trades"
  -H "Authorization: bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
```

> The above command returns JSON structured like this:

```json
{
    "data": [
        {
            "_id": "5bbd1c6709ac22627841ad32",
            "createdAt": "2018-10-09T21:23:51.757Z",
            "price": 513.3,
            "quantity": 0.1,
            "side": "Buy",
            "user": {
                "_id": "5ab545a4b933aa1f78e25f34",
                "firstName": "Roger",
                "lastName": "Ver"
            },
            "asset": "BCH",
            "status": "Complete"
        }
    ],
    "total": 1,
    "pageCount": 1
}
```

This endpoint retrieves all trades.

### HTTP Request

`GET https://sandbox.otc.dvchain.co/api/v4/trades`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
before | none | If set, will only return trades before this date/time.
after | none |  If set, will only return trades after this date/time.
page | none | If set, it will return the given page number of trades.
limit | none | If set, it will limit the number of trades returned per page.

## Cancel a Limit Order

```bash
curl "https://sandbox.otc.dvchain.co/api/v4/trades/:tradeId" \
  -X DELETE \
  -H "Content-Type: application/json" \
  -H "Authorization: bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
```

> The above command returns JSON structured like this:

```json
{
    "message": "Trade successfully cancelled."
}
```

This endpoint cancels an open limit order.

### HTTP Request

`DELETE https://sandbox.otc.dvchain.co/api/v4/trades/:tradeId`

