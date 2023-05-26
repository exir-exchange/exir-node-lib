# exir-node-lib

Nodejs library for exir Kit.

**This library is specifically for end users and traders to connect to Exir. It connects to [Exir](https://pro.exir.io/trade/btc-usdt) by default.**

## Usage

```javascript
const exir = require('exir-node-lib');

const client = new exir();
```

You can pass custom `apiURL`, `wsURL` and `baseURL` of the Exir to connect to. `apiURL` is `https://api.exir.io`
`wsURL` is the websocket URL for the socket connection and you should pass your stream URL. For Exir it is `wss://api.exir.io/stream`. `baseURL` is not required and it is set by default to `/v2` unless you need to connect to an older version of Exir.

You can also pass your `apiKey` and `apiSecret` generated from the exchange to use private requests that require authentication. For public endpoints `apiKey` and `apiSecret` are not required.

```javascript
const client = new exir({
	apiURL: '<EXCHANGE_API_URL>',
	wsURL: '<EXCHANGE_WS_URL>',
	apiKey: '<MY_API_KEY>',
	apiSecret: '<MY_API_SECRET>'
});
```

You can also pass the field `apiExpiresAfter` which is the length of time in seconds each request is valid for. The default value is `60`.

### Example:

```javascript
const client = new exir({
	apiURL: '<EXCHANGE_API_URL>',
	wsURL: '<EXCHANGE_API_URL>',
	apiKey: '<MY_API_KEY>',
	apiSecret: '<MY_API_SECRET>'
});

client
	.getTicker('btc-usdt')
	.then((res) => {
		console.log('The volume is: ', res.volume);
	})
	.catch((err) => {
		console.log(err);
	});

client
	.getTrades({ symbol: 'btc-usdt' })
	.then((res) => {
		console.log('Public trades: ', res);
	})
	.catch((err) => {
		console.log(err);
	});
```

### Available functions:

| Command | Parameters | Description |
| - | - | - |
| `getKit` | | Get exchange information e.g. name, valid languages, description, etc. |
| `getConstants` | | Tick size, min price, max price, min size and max size of each symbol pair and coin |
| `getTicker` | <ul><li>**symbol**: Exir trading symbol e.g. `btc-usdt`</li></ul> | Last, high, low, open and close price and volume within the last 24 hours |
| `getTickers` | | Last, high, low, open and close price and volume within the last 24 hours for all symbols |
| `getOrderbook` | <ul><li>**symbol**: Exir trading symbol e.g. `btc-usdt`</li></ul> | Orderbook containing list of bids and asks |
| `getOrderbooks` | | Orderbook containing list of bids and asks for all symbols |
| `getTrades` | <ul><li>**opts**: Object with additional params</li><li>**opts.symbol**: (_optional_) Exir trading symbol e.g. `btc-usdt`</li></ul> | List of last trades |
| `getUser` | | User's personal information |
| `getBalance` | | User's wallet balance |
| `getDeposits` | <ul><li>**opts**: Object with additional params</li><li>**opts.currency**: (_optional_) Filter data set by asset</li><li>**opts.status**: (_optional_) Filter data set `status`</li><li>**opts.dismissed**: (_optional_) Filter data set `dismissed`</li><li>**opts.rejected**: (_optional_) Filter data set `rejected`</li><li>**opts.processing**: (_optional_) Filter data set `processing`</li><li>**opts.waiting**: (_optional_) Filter data set `waiting`</li><li>**opts.limit**: (_optional_, _default_=`50`, _max_=`50`) Number of items to get</li><li>**opts.page**: (_optional_, _default_=`1`) Page number of data</li><li>**opts.orderBy**: (_optional_) Field to order data by</li><li>**opts.order**: (_optional_, _enum_=[`asc`, `desc`]) Specify ascending or descending order</li><li>**opts.startDate**: (_optional_, _format_=`ISO8601`) Start date of data set</li><li>**opts.endDate**: (_optional_,  _format_=`ISO8601`) End date of data set</li><li>**opts.transactionId**: (_optional_) Filter data set by TXID</li><li>**opts.address**: (_optional_) Filter data set by address</li></ul> | User's list of all deposits |
| `getWithdrawals` | <ul><li>**opts**: Object with additional params</li><li>**opts.currency**: (_optional_) Filter data set by asset</li><li>**opts.status**: (_optional_) Filter data set `status`</li><li>**opts.dismissed**: (_optional_) Filter data set `dismissed`</li><li>**opts.rejected**: (_optional_) Filter data set `rejected`</li><li>**opts.processing**: (_optional_) Filter data set `processing`</li><li>**opts.waiting**: (_optional_) Filter data set `waiting`</li><li>**opts.limit**: (_optional_, _default_=`50`, _max_=`50`) Number of items to get</li><li>**opts.page**: (_optional_, _default_=`1`) Page number of data</li><li>**opts.orderBy**: (_optional_) Field to order data by</li><li>**opts.order**: (_optional_, _enum_=[`asc`, `desc`]) Specify ascending or descending order</li><li>**opts.startDate**: (_optional_, _format_=`ISO8601`) Start date of data set</li><li>**opts.endDate**: (_optional_,  _format_=`ISO8601`) End date of data set</li><li>**opts.transactionId**: (_optional_) Filter data set by TXID</li><li>**opts.address**: (_optional_) Filter data set by address</li></ul> | User's list of all withdrawals |
| `makeWithdrawal` | <ul><li>**currency**: Currency code e.g. `btc`</li><li>**amount**: Withdrawal amount</li><li>**address**: Address to withdrawal to</li><li>**opts**: Object with additional params</li><li>**opts.network**: (_required if asset has multiple networks_) Blockchain network to create address for e.g. `trx`</li></ul> | Create a new withdrawal request |
| `getUserTrades` | <ul><li>**opts**: Object with additional params</li><li>**opts.symbol**: (_optional_) Exir trading symbol e.g. `btc-usdt`</li><li>**opts.limit**: (_optional_, _default_=`50`, _max_=`50`) Number of items to get</li><li>**opts.page**: (_optional_, _default_=`1`) Page number of data</li><li>**opts.orderBy**: (_optional_) Field to order data by</li><li>**opts.order**: (_optional_, _enum_=[`asc`, `desc`]) Specify ascending or descending order</li><li>**opts.startDate**: (_optional_, _format_=`ISO8601`) Start date of data set</li><li>**opts.endDate**: (_optional_,  _format_=`ISO8601`) End date of data set</li></ul> | User's list of all trades |
| `getOrder` | <ul><li>**orderId**: Exir Network Order ID</li></ul> | Get specific information about a certain order |
| `getOrders` | <ul><li>**opts**: Object with additional params</li><li>**opts.symbol**: (_optional_) Exir trading symbol e.g. `btc-usdt`</li><li>**opts.side**: (_optional_, _enum_=[`buy`, `sell`]) Order side</li><li>**opts.status**: (_optional_) Filter data set `status`</li><li>**opts.limit**: (_optional_, _default_=`50`, _max_=`50`) Number of items to get</li><li>**opts.page**: (_optional_, _default_=`1`) Page number of data</li><li>**opts.orderBy**: (_optional_) Field to order data by</li><li>**opts.order**: (_optional_, _enum_=[`asc`, `desc`])</li><li>**opts.startDate**: (_optional_, _format_=`ISO8601`) Start date of data set</li><li>**opts.endDate**: (_optional_,  _format_=`ISO8601`) End date of data set</li></ul> | Get the list of all user orders. It can be filter by passing the symbol |
| `createOrder` | <ul><li>**symbol**: Exir trading symbol e.g. `btc-usdt`</li><li>**side** (_enum_=[`buy`, `sell`]): Order side</li><li>**size**: Size of order to place</li><li>**type**: (_enum_=[`market`, `limit`] Order type</li><li>**price**: (_required if limit order type_) Order price</li><li>**opts**: Object with additional params</li><li>**opts.stop**: (_optional_) Stop price for order</li><li>**opts.meta**: (_optional_) Object with additional meta configurations</li><li>**opts.meta.post_only**: (_optional_, _default_=`false`) Make post only order </li><li>**opts.meta.note**: (_optional_) Custom note for order</li></ul> | Create a new order |
| `cancelOrder` | <ul><li>**orderId**: Exir Network order ID</li></ul> | Cancel a specific order with its ID |
| `cancelAllOrders` | <ul><li>**symbol**: Exir trading symbol e.g. `btc-usdt`</li></ul> | Cancel all the active orders of a user, filtered by currency pair symbol |


### Websocket

#### Functions

You can connect and subscribe to different websocket channels for realtime updates.

To connect, use the `connect` function with the channels you want to subscribe to in an array as the parameter. The connection will reconnect on it's own unless you call `disconnect`.

```javascript
client.connect(['orderbook', 'trade']);
```

To disconnect the websocket, call `disconnect`.

```javascript
client.disconnect();
```

To subscribe to more channels after connection, use `subscribe`.

```javascript
client.subscribe(['order', 'wallet']);
```

To unsubscribe from channels after connection, use `unsubscribe`.

```javascript
client.unsubscribe(['orderbook']);
```

#### Channels

Here is the list of channels you can subscribe to:

- `orderbook` (Available publicly)
- `trade` (Available publicly)
- `order` (Only available with authentication. Receive order updates)
- `usertrade` (Only available with authentication. Receive user trades)
- `wallet` (Only available with authentication. Receive balance updates)
- `deposit` (Only available with authentication. Receive deposit notifications)
- `withdrawal` (Only available with authentication. Receive withdrawal notifications)


For public channels (`orderbook`, `trade`), you can subscribe to specific symbols as follows:
`orderbook:btc-usdt`, `trade:btc-usdt`. Not passing a symbol will subscribe to all symbols.

#### Events

After connecting to the websocket, you can listen for events coming from the server by using the `on` function for the `ws` property of the client.
The events available are default websocket events e.g. `message`, `open`, `close`, `error`, `unexpected-response`, etc.

```javascript
client.ws.on('message', (data) => {
	data = JSON.parse(data);
	console.log(data);
});
```

These are exapmles of data responses from the server.

- **orderbook**: Updates related to the user's private information are as follows:

	```json
	{
		"topic": "orderbook",
		"action": "partial",
		"symbol": "btc-usdt",
		"data": {
			"bids": [
				[0.1, 0.1],
				...
			],
			"asks": [
				[1, 1],
				...
			],
			"timestamp": "2020-12-15T06:45:27.766Z"
		},
		"time": 1608015328
	}
	```

- **trade**: Updates related to the user's private information are as follows:

	```json
	{
		"topic": "trade",
		"action": "partial",
		"symbol": "btc-usdt",
		"data": [
			{
				"size": 0.012,
				"price": 300,
				"side": "buy",
				"timestamp": "2020-12-15T07:25:28.887Z"
			},
			...
		],
		"time": 1608015328
	}
	```

- **wallet**: Updates related to the user's private information are as follows:

	```json
	{
		"topic": "wallet",
		"action": "partial",
		"user_id": 1,
		"data": {
			"usdt_balance": 1,
			"usdt_available": 1,
			"btc_balance": 1,
			"btc_available": 1,
			"xmr_balance": 1,
			"xmr_available": 1,
			"btc_balance": 1,
			"btc_available": 1,
			"eth_balance": 1,
			"eth_available": 1,
			...,
			"updated_at": "2020-12-15T08:41:24.048Z"
		},
		"time": 1608021684
	}
	```

- **order**: Websocket messages relating the the user's orders.
    - The `status` of the order can be `new`, `pfilled`, `filled`, and `canceled`.
    - The `action` of the data determines what caused it to happen. All three are explained below:

  - `partial`: All previous and current orders. Is the first order data received when connecting. Max: 50. Descending order.

	```json
	{
		"topic": "order",
		"action": "partial",
		"user_id": 1,
		"data": [
			{
				"id": "7d3d9545-b7e6-4e7f-84a0-a39efa4cb173",
				"side": "buy",
				"symbol": "btc-usdt",
				"type": "limit",
				"size": 0.1,
				"filled": 0,
				"price": 1,
				"stop": null,
				"status": "new",
				"fee": 0,
				"fee_coin": "btc",
				"meta": {},
				"fee_structure": {
					"maker": 0.1,
					"taker": 0.1
				},
				"created_at": "2020-11-30T07:45:43.819Z",
				"created_by": 1
			},
			...
		],
		"time": 1608022610
	}
	```

  - `insert`: When user's order is added. The status of the order can be either `new`, `pfilled`, or `filled`.

	```json
  	{
		"topic": "order",
		"action": "insert",
		"user_id": 1,
		"symbol": "btc-usdt",
		"data": [
			{
				"id": "7d3d9545-b7e6-4e7f-84a0-a39efa4cb173",
				"side": "buy",
				"symbol": "btc-usdt",
				"type": "limit",
				"size": 0.1,
				"filled": 0,
				"price": 1,
				"stop": null,
				"status": "new",
				"fee": 0,
				"fee_coin": "btc",
				"meta": {},
				"fee_structure": {
					"maker": 0.1,
					"taker": 0.1
				},
				"created_at": "2020-11-30T07:45:43.819Z",
				"updated_at": "2020-12-15T08:56:45.066Z",
				"created_by": 1
			},
			...
		],
		"time": 1608022610
	}
	```

  - `update`: When user's order status is updated. Status can be `pfilled`, `filled`, and `canceled`.

	```json
  	{
		"topic": "order",
		"action": "insert",
		"user_id": 1,
		"symbol": "btc-usdt",
		"data": [
			{
				"id": "7d3d9545-b7e6-4e7f-84a0-a39efa4cb173",
				"side": "buy",
				"symbol": "btc-usdt",
				"type": "limit",
				"size": 0.1,
				"filled": 0,
				"price": 1,
				"stop": null,
				"status": "new",
				"fee": 0,
				"fee_coin": "btc",
				"meta": {},
				"fee_structure": {
					"maker": 0.1,
					"taker": 0.1
				},
				"created_at": "2020-11-30T07:45:43.819Z",
				"updated_at": "2020-12-15T08:56:45.066Z",
				"created_by": 1
			},
			...
		],
		"time": 1608022610
	}
	```
- **deposit**: Updates related to the user's private information are as follows:

	```json
	{
		"topic": "deposit",
		"action": "insert",
		"user_id": 1,
		"data": {
			"amount": 1,
			"currency": "btc",
			"status": "COMPLETED",
			 "transaction_id": "123",
			...
		},
		"time": 1608021684
	}
	```
- **withdrawal**: Updates related to the user's private information are as follows:

	```json
	{
		"topic": "withdrawal",
		"action": "insert",
		"user_id": 1,
		"data": {
			"amount": 1,
			"currency": "btc",
			"status": "COMPLETED",
			 "transaction_id": "123",
			...
		},
		"time": 1608021684
	}
	```

## Example

You can run the example by going to example folder and running:

```bash
node example/exir.js
```

## Documentation

For adding additional functionalities simply go to index.js and add more features.
You can read more about api documentation at https://apidocs.exir.io
You should create your token on the platform in setting->api keys
