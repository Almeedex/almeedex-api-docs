# Public Rest API for Almeedex
# General API Information
* The base endpoint is: **http://almeedex.com**
* All endpoints return either a JSON object or array.
* Data is returned in **ascending** order. Oldest first, newest last.
* All time and timestamp related fields are in milliseconds.
* HTTP `4XX` return codes are used for for malformed requests;
  the issue is on the sender's side.
* HTTP `429` return code is used when breaking a request rate limit.
* HTTP `418` return code is used when an IP has been auto-banned for continuing to send requests after receiving `429` codes.
* HTTP `5XX` return codes are used for internal errors; the issue is on
  Almeedex's side.
  It is important to **NOT** treat this as a failure operation; the execution status is
  **UNKNOWN** and could have been a success.
* Any endpoint can return an ERROR; the error payload is as follows:
```javascript
{
  "code": -1121,
  "msg": "Invalid symbol."
}
```

* Specific error codes and messages defined in another document.
* For `GET` endpoints, parameters must be sent as a `query string`.
* Parameters may be sent in any order.
* If a parameter sent in both the `query string` and `request body`, the
  `query string` parameter will be used.


# Public API Endpoints

### Exchange information
```
GET /api/v1/exchangeInfo
```
Current exchange trading rules and symbol information

**Parameters:**
NONE

**Response:**
```javascript
{
  "timezone": "UTC",
  "serverTime": 1561350129,
  "symbols": [
    {
      "symbol": "ALDXKZE",
      "status": "TRADING",
      "baseAsset": "ALDX",
      "baseAssetPrecision": 8,
      "quoteAsset": "KZE",
      "quotePrecision": 8,
      "orderTypes": [
        "LIMIT"
      ]
    }
}
```

## Market Data endpoints

### 24hr ticker price change statistics
```
GET /api/v1/ticker
```
24 hour rolling window price change statistics. 



**Response:**
```javascript
{
  {
    "pair": "ETH/USDT",
    "high": "320.48460000",
    "low": "305.33550000",
    "close": "306.17110000",
    "open": "312.62500000",
    "ask": "314.57250000",
    "bid": "299.18850000",
    "volume": "47470.50",
    "coin": "USDT",
    "dest": "ETH",
    "timestamp": 1561349797
  },
  {
    "pair": "BTC/USDT",
    "high": "11346.18110000",
    "low": "10549.38350000",
    "close": "10828.95150000",
    "open": "10666.82910000",
    "ask": "10931.86630000",
    "bid": "10715.37390000",
    "volume": "333119.79",
    "coin": "USDT",
    "dest": "BTC",
    "timestamp": 1561349797
  },
  ...
}
```

### Trading data
```
GET /api/v1/depth
```

**Parameters:**

Name | Type | Required | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
limit | INT | NO | Default 5; max 1000. Valid limits:[5, 10, 20, 50, 100, 500, 1000]


**Response:**
```javascript
{
  "lastUpdateId": 1027024,
  "bids": [
    [
      "4.00000000",     // PRICE
      "431.00000000"    // QTY
    ]
  ],
  "asks": [
    [
      "4.00000200",
      "12.00000000"
    ]
  ]
}
```

### Order book data
```
GET /api/v1/order_book
```
This API gets the order book of a specified market. Both asks and bids are sorted from highest price to lowest.


**Parameters:**

Name | Type | Required | Description
------------ | ------------ | ------------ | ------------
market | STRING | YES |
limit | INT | NO | Default 5; max 1000. Valid limits:[5, 10, 20, 50, 100, 500, 1000]


**Response:**
```javascript
{
  "code": 0,
  "msg": "Operation is successful",
  "data": {
    "timestamp": 1561349306,
    "bids": [
      [
        "10715.37390000",
        "0.00466619"
      ]
    ],
    "asks": [
      [
        "10938.03740000",
        "0.00900000"
      ]
    ]
  }
}
```

### Get coins
```
GET /api/v1/coins
```
This endpoint retrieves a list of all tokens. Not all tokens can be used for trading. Tokens which have or had no representation in ISO 4217 may use a custom code.




**Response:**
```javascript
[
  {
    "coin": "ETH",
    "can_deposit": 1,
    "can_withdraw": 1,
    "min_trade": "0.00100000",
    "min_deposit": "0.01500000",
    "min_withdraw": "0.03000000",
    "trade_fee": "0.00100000",
    "deposit_fee": "0.00000000",
    "withdraw_fee": "0.01000000"
  }
]
```


### Market history
```
GET /api/v1/historicalTrades
```
Get older trades.


**Parameters:**

Name | Type | Required | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
limit | INT | NO | Default 5; max 1000.

**Response:**
```javascript
[
  {
    "id": 28457,
    "price": "4.00000100",
    "amount": "12.00000000",
    "time": 1499865549590,
  }
]
```

