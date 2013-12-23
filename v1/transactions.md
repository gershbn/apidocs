---
title: Transactions | OANDA API
---

# Transaction Endpoints

* TOC
{:toc}

## Get transaction history

    GET /v1/accounts/:account_id/transactions

#### Input Query Parameters

maxId
: _Optional_ First transaction to get. The server will return transactions with id less than or equal to this, in descending order (for pagination). 

minId
: _Optional_ Last transaction to get. The server will return transactions with id greater or equal to this, in descending order (for pagination).

count
: _Optional_ Maximum number of transactions to return. The default and maximum value for count is 50.

instrument
: _Optional_ Retrieve transactions for a specific instrument only. Default: all 

ids
: _Optional_ A comma separated list of transactions ids to retrieve. Maximum number of ids: 50. No other parameter may be specified with the ids parameter.s

#### Example
    curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/transactions?instrument=EUR_USD&count=1"

#### Response


###### Header

~~~Header
Link: <http://api-sandbox.oanda.com/v1/accounts/6531071/transactions?count=1&maxId=17780896>; ref="next"
~~~

###### Body

~~~json
{
  "transactions" : [
    {
      "id" : 177808963,
      "accountId" : 6531071,
      "type" : "marketIfTouched",
      "units" : 2,
      "side" : "sell",
      "action" : "open",
      "reason" : "user_submitted",
      "instrument" : "EUR_USD",
      "time" : "2012-07-03T14:30:38Z",
      "price" : 1.2736,
      "balance" : 100000.0815,
      "profitLoss" : 0,
      "marginUsed" : 0.1274
    },
  ]
}
~~~

####Response Fields

Transactions returned may be of different types describing an event that happened to the account.
Transaction of each type contains only relevant sub-set of fields. The following fields are the most common: 

id
: Transaction ID

accountId
: Account ID of the user

time
: Date and time in RFC3339 format (http://www.ietf.org/rfc/rfc3339.txt)

type
: Possible types are: MARKET_ORDER_CREATE , STOP_ORDER_CREATE, LIMIT_ORDER_CREATE, MARKET_IF_TOUCHED_ORDER_CREATE, ORDER_UPDATE, TRADE_UPDATE, TRADE_CLOSE, ORDER_CANCEL, ORDER_FILLED, STOP_LOSS_FILLED, TAKE_PROFIT_FILLED, TRAILING_STOP_FILLED, MARGIN_CALL_ENTER, MARGIN_CALL_EXIT, MARGIN_CLOSEOUT, MIGRATE_TRADE_OPEN, MIGRATE_TRADE_CLOSE, SET_MARGIN_RATE, TRANSFER_FUNDS, DAILY_INTEREST, FEE

instrument
: An instrument

side
: Direction of the action performed on the account, possible values are: buy, sell

units
: Amount of units involved 

price
: Execution or requested price

lowerBound
: Minimum execution price

upperBound
: Maximum execution price

takeProfitPrice
: Price of the Take Profit

stopLossPrice
: Price of the Stop Loss

trailingStopLossDistance
: Distance of the Trailing Stop in pips, up to one decimal place

pl
: Profit and Loss value

interest
: Interest accrued

accountBalance
: Balance on the account after the event

####Description of transaction types and a sub-set of corresponding fields

#####MARKET_ORDER_CREATE
A transaction of this type created when a user has successfully traded a specified number of units of an instrument at the current market price.

Required Fields
: id, accountId, time, type, instrument, side, units, price, pl, interest, accountBalance

Optional Fields
: lowerBound, upperBound, takeProfitPrice, stopLossPrice, trailingStopLossDistance

######Trade information
Transaction of a MARKET_ORDER_CREATE type also contains information about associated trade

tradeOpened
: This object is appended to the json response if a new trade has been opened. Trade related fields are: id, units

tradeReduced
: This object is appended to the json response if a trade has been closed or reduced. Trade related fields are: id, units, pl, interest


####Pagination

Transactions can be paginated with the count and maxId parameters.
At most, a maximum of 50 transactions can be returned in one query. 
If more transactions exist than specified by the given or default count, a url with maxId set to the next unreturned transaction will be returned within the Link header.

----

## Get information for a transaction

    GET /v1/accounts/:account_id/transactions/:transaction_id

#### Example
    curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/transactions/1170980"

#### Response

~~~json
{
  "id" : 177808963,
  "accountId" : 6531071,
  "type" : "marketIfTouched",
  "units" : 2,
  "side" : "sell",
  "action" : "open",
  "reason" : "user_submitted",
  "instrument" : "EUR_USD",
  "time" : "2012-07-03T14:30:38Z",
  "price" : 1.2736,
  "balance" : 100000.0815,
  "profitLoss" : 0,
  "marginUsed" : 0.1274
}
~~~
