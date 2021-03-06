# Websocket Authenticated Channels
* [New Order](#new-order-1)
* [Cancel Order](#cancel-order-1)
* [Order Update](#order-update)
* [Account Update](#account-update)

<a name="new-order-1" id="new-order-1"> </a>

## New Order

### Request Fields

Name                   | Type(value)          | Mandatory   | Description
-----------------------| ---------------------|-------------|-----------------------
apiKey                 | String               | M           | 
signature              | String               | M           | 
passPhrase             | String               | M           | 
type                   | newOrder             | M           | 
instrumentId           | String               | M           | 
userAccount            | String               | M           | User account used for this order
orderType              | String               | M           | Limit/Market/Stop/StopLimit
price                  | DoubleString         | (M)         | Mandatory if order is non Market order
size                   | DoubleString         | (M)         | Mandatory if order is non Market order;For Market order, the user can either provide size or amount in quote currency
amount                 | DoubleString         | O           | Amount in quote currency to buy/sell
side                   | Buy/Sell             | M           | 
timeInForce            | GTC, GTT, IOC FOK    | M           | 
clientOrderId          | String               | O           | 
stopPrice              | NumberString         | (M)         | Mandatory if orderType is Stop or StopLimit
postOnly               | Boolean              | O           | Default to false. If any part of the order results in taking liquidity, the order will be rejected and no part of it will execute.
selfTradePrevention    | String               | O           | Decrease and cancel (Default);Cancel oldest;Cancel newest;Cancel both;
userMessageId          | Integer              | O           | Unique message id for this websocket session


### Failure Error Codes

Error Code            | Description
----------------------| -----------------


<a name="cancel-order-1" id="cancel-order-1"> </a>


## Cancel Order

### Request Fields

Name                   | Type(value)          | Mandatory   | Description
-----------------------| ---------------------|-------------|--------------------
apiKey                 | String               | M           | 
signature              | String               | M           | 
passPhrase             | String               | M           | 
type                   | cancelOrder          | M           | 
orderId                | String               | (M)         | 
clientOrderId          | String               | (M)         | 
userMessageId          | Integer              | O           | Unique message id for this websocket session


### Failure Error Codes

Error Code            | Description
----------------------| -----------------


<a name="order-update" id="order-update"> </a>


## Order Update

### Request Fields

Name                       | Type(value)                    | Mandatory   | Description
---------------------------| -------------------------------| ------------|--------------------
apiKey                     | String                         | M           | 
signature                  | String                         | M           | 
passPhrase                 | String                         | M           | 
type                       | subscribe                      | M           | Subscribe/Unsubscribe message type
channel                    | orderUpdate                    | M           | 
instrumentIds              | Array of instrumentId Strings  | O           | Subscribe/Unsubscribe all instruments by default
numberOfSnapshotRecords    | Integer                        | O           | Max recent number of open orders returned. All open orders will be returned if not specified.
userMessageId              | Integer                        | O           | Unique message id for this websocket session


### Response Fields

Name            | Type(value)                    | Mandatory   | Description
----------------| -------------------------------| ------------|--------------------
type            | subscribed                     | M           | Subscribe/Unsubscribe message type
channel         | orderUpdate                    | M           | 
instrumentIds   | Array of instrumentId Strings  | O           | Subscribed/Unsubscribed for all instruments by default
openOrders      | Array of orders Json Fields    | M           | 
userMessageId   | Integer                        | (M)         | Mandatory if userMessageId is provided in the user request


### Channel Fields

Name                 | Type(value)                               | Mandatory   | Description
---------------------| ------------------------------------------| ------------|--------------------
type                 | orderUpdate                               | M           | 
orderId              | String                                    | M           | 
subaccount           | String                                    | M           | 
instrumentId         | String                                    | M           | 
type                 | Market, Limit, Stop, StopLimit            | M           | 
price                | DoubleString                              | M           | 
size                 | DoubleString                              | M           | 
side                 | Buy/Sell                                  | M           | 
timeInForce          | GTC, GTT, IOC, FOK                        | M           | 
orderTime            | Timestamp                                 | M           | 
stopPrice            | DoubleString                              | O           | 
status               | Open,Partially Filled, Filled, Cancelled  | M           | 
executedSize         | DoubleString                              | O           | 
executedPrice        | DoubleString                              | O           | 
postLiquidity        | Boolean                                   | M           | True – order rest in the book initially 
fee                  | DoubleString                              | O           | 
selfTradePrevention  | String                                    | M           | DC – Decrease and cancel;CO – Cancel oldest;CN – Cancel newest;CB – Cancel both
clientOrderId        | String                                    | O           | 


### Failure Error Codes

Error Code            | Description
----------------------| -----------------


<a name="account-update" id="account-update"> </a>


## Account Update

### Request Fields

Name                         | Type(value)                    | Mandatory   | Description
-----------------------------| -------------------------------| ------------|--------------------
apiKey                       | String                         | M           | 
signature                    | String                         | M           | 
type                         | subscribe                      | M           | Subscribe/Unsubscribe message type
channel                      | accountUpdate                  | M           | 
currencies                   | Array of currency Strings      | O           | Subscribe for all currencies by default
numberOfRecentTransactions   | Integer                        | O           | Default to 100
userMessageId                | Integer                        | O           | Unique message id for this websocket session


### Response Fields

Name                   | Type(value)                                                                   | Mandatory   | Description
-----------------------| ------------------------------------------------------------------------------| ------------|--------------------
type                   | subscribed                                                                    | M           | Subscribe/Unsubscribe message type
channel                | accounts                                                                      | M           | 
currencies             | Array of currency Strings                                                     | O           | 
accountBalances        | Array of account balances (accountId, currency, balance, availableBalance)    | M           | 
recentAccountUpdates   | Array of recent accountUpdate Json fields                                     | M           | 
userMessageId          | Integer                                                                       | (M)         | Mandatory if userMessageId is provided in the user request

### Channel Fields

Name              | Type(value)       | Mandatory   | Description
------------------| ------------------| ------------|--------------------
type              | accountUpdate     | M           | 
userAccount       | String            | M           | 
transactionId     | String            | M           | 
accountId         | String            | M           | 
currency          | String            | M           | 
transactionType   | String            | M           | Trade/Deposit/Withdraw/Fee/Rebate/Order/PendingWithdraw
amount            | DoubleString      | M           | 
balance           | DoubleString      | M           | 
available         | DoubleString      | M           | 
timestamp         | Timestamp         | M           | 
referenceId       | String            | M           | Additional information;Trade – {tradeId};Withdraw – {withdrawId};Fee – {tradeId or withdrawId};Order – {orderId};WithdrawRequest – {withdrawId}


### Failure Error Codes

Error Code            | Description
----------------------| -----------------