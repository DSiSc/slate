---
title: RPC Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - go

toc_footers:
  - <a href='http://www.codeaslaw.io/'>Justitia</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

search: true
---

# Introduction

Justitia RPC is built using our own RPC library which contains its own set of documentation and tests.
See it here: <a href="https://github.com/DSiSc/apigateway/tree/master/rpc/lib">https://github.com/DSiSc/apigateway/tree/master/rpc/lib</a>

Or resort to the rpc documentation at: <a href="https://dsisc.github.io/slate/">https://dsisc.github.io/slate/</a>

# Endpoints




## [BlockNumber](https://github.com/DSiSc/apigateway/tree/master/rpc/core/block.go?s=6293:6332#L235)
#### eth_blockNumber

Returns the number of most recent block.

##### Parameters
none

##### Returns

`QUANTITY` - integer of the current block number the client is on.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":83}'

// Result
{


	"id":83,
	"jsonrpc": "2.0",
	"result": "0xc94" // 1207

}
```

***

## [Call](https://github.com/DSiSc/apigateway/tree/master/rpc/core/tx.go?s=18002:18081#L627)
#### eth_call

Executes a new message call immediately without creating a transaction on the block chain.

##### Parameters

1. `Object` - The transaction call object
- `from`: `DATA`, 20 Bytes - (optional) The address the transaction is sent from.
- `to`: `DATA`, 20 Bytes  - The address the transaction is directed to.
- `gas`: `QUANTITY`  - (optional) Integer of the gas provided for the transaction execution. eth_call consumes zero gas, but this parameter may be needed by some executions.
- `gasPrice`: `QUANTITY`  - (optional) Integer of the gasPrice used for each paid gas
- `value`: `QUANTITY`  - (optional) Integer of the value sent with this transaction
- `data`: `DATA`  - (optional) Hash of the method signature and encoded parameters. For details see [Ethereum Contract ABI](<a href="https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI">https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI</a>)
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter)

##### Returns

`DATA` - the return value of executed contract.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_call","params":[{see above}],"id":1}'

// Result
{


	"id":1,
	"jsonrpc": "2.0",
	"result": "0x"

}
```

***

## [EstimateGas](https://github.com/DSiSc/apigateway/tree/master/rpc/core/tx.go?s=21773:21833#L777)
#### eth_estimateGas

Generates and returns an estimate of how much gas is necessary to allow the transaction to complete. The transaction will not be added to the blockchain. Note that the estimate may be significantly more than the amount of gas actually used by the transaction, for a variety of reasons including EVM mechanics and node performance.

##### Parameters

See [eth_call](#eth_call) parameters, expect that all properties are optional. If no gas limit is specified geth uses the block gas limit from the pending block as an upper bound. As a result the returned estimate might not be enough to executed the call/transaction when the amount of gas is higher than the pending block gas limit.

##### Returns

`QUANTITY` - the amount of gas used.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_estimateGas","params":[{see above}],"id":1}'

// Result
{


	"id":1,
	"jsonrpc": "2.0",
	"result": "0x5208" // 21000

}
```

***

## [GasPrice](https://github.com/DSiSc/apigateway/tree/master/rpc/core/tx.go?s=20639:20672#L747)
#### eth_gasPrice

Returns the current price per gas in wei.

##### Parameters
none

##### Returns

`QUANTITY` - integer of the current gas price in wei.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_gasPrice","params":[],"id":73}'

// Result
{


	"id":73,
	"jsonrpc": "2.0",
	"result": "0x09184e72a000" // 10000000000000

}
```

***

## [GetBalance](https://github.com/DSiSc/apigateway/tree/master/rpc/core/block.go?s=7363:7452#L280)
#### eth_getBalance

Returns the balance of the account of given address.

##### Parameters

1. `DATA`, 20 Bytes - address to check for balance.
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter)

```js
params: [


	'0xc94770007dda54cF92009BFF0dE90c06F603a09f',
	'latest'

]
```

##### Returns

`QUANTITY` - integer of the current balance in wei.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBalance","params":["0xc94770007dda54cF92009BFF0dE90c06F603a09f", "latest"],"id":1}'

// Result
{


	"id":1,
	"jsonrpc": "2.0",
	"result": "0x0234c8a3397aab58" // 158972490234375000

}
```

***

## [GetBlockByHash](https://github.com/DSiSc/apigateway/tree/master/rpc/core/block.go?s=2318:2399#L65)
#### eth_getBlockByHash

Returns information about a block by hash.

##### Parameters

1. `DATA`, 32 Bytes - Hash of a block.
2. `Boolean` - If `true` it returns the full transaction objects, if `false` only the hashes of the transactions.

```js
params: [


	'0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331',
	true

]
```

##### Returns

`Object` - A block object, or `null` when no block was found:

- `number`: `QUANTITY` - the block number. `null` when its pending block.
- `hash`: `DATA`, 32 Bytes - hash of the block. `null` when its pending block.
- `parentHash`: `DATA`, 32 Bytes - hash of the parent block.
- `transactionsRoot`: `DATA`, 32 Bytes - the root of the transaction trie of the block.
- `stateRoot`: `DATA`, 32 Bytes - the root of the final state trie of the block.
- `receiptsRoot`: `DATA`, 32 Bytes - the root of the receipts trie of the block.
- `miner`: `DATA`, 20 Bytes - the address of the beneficiary to whom the mining rewards were given.
- `timestamp`: `QUANTITY` - the unix timestamp for when the block was collated.
- `transactions`: `Array` - Array of transaction objects, or 32 Bytes transaction hashes depending on the last given parameter.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockByHash","params":["0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331", true],"id":1}'

// Result
{
"id":1,
"jsonrpc":"2.0",
"result": {


	  "number": "0x1b4", // 436
	  "hash": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331",
	  "parentHash": "0x9646252be9520f6e71339a8df9c55e4d7619deeb018d2a3f2d21fc165dde5eb5",
	  "transactionsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
	  "stateRoot": "0xd5855eb08b3387c0af375e9cdb6acfc05eb8f519e419b874b6ff2ffda7ed1dff",
	  "miner": "0x4e65fda2159562a496f9f3522f89122a3088497a",
	  "timestamp": "0x54e34e8e" // 1424182926
	  "transactions": [{...},{ ... }]
	}

}
```

***

## [GetBlockByNumber](https://github.com/DSiSc/apigateway/tree/master/rpc/core/block.go?s=5406:5499#L192)
#### eth_getBlockByNumber

Returns information about a block by block number.

##### Parameters

1. `QUANTITY|TAG` - integer of a block number, or the string `"earliest"`, `"latest"` or `"pending"`, as in the [default block parameter](#the-default-block-parameter).
2. `Boolean` - If `true` it returns the full transaction objects, if `false` only the hashes of the transactions.

```js
params: [


	'0x1b4', // 436
	true

]
```

##### Returns

See [eth_getBlockByHash](#eth_getblockbyhash)

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["0x1b4", true],"id":1}'
```

Result see [eth_getBlockByHash](#eth_getblockbyhash)

***

## [GetBlockTransactionCountByHash](https://github.com/DSiSc/apigateway/tree/master/rpc/core/block.go?s=3334:3408#L111)
#### eth_getBlockTransactionCountByHash

Returns the number of transactions in a block from a block matching the given block hash.

##### Parameters

1. `DATA`, 32 Bytes - hash of a block.

```js
params: [


	'0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238'

]
```

##### Returns

`QUANTITY` - integer of the number of transactions in this block.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockTransactionCountByHash","params":["0xc94770007dda54cF92009BFF0dE90c06F603a09f"],"id":1}'

// Result
{


	"id":1,
	"jsonrpc": "2.0",
	"result": "0xc" // 11

}
```

***

## [GetBlockTransactionCountByNumber](https://github.com/DSiSc/apigateway/tree/master/rpc/core/block.go?s=4345:4431#L153)
#### eth_getBlockTransactionCountByNumber
> >
Returns the number of transactions in a block matching the given block number.

##### Parameters

1. `QUANTITY|TAG` - integer of a block number, or the string `"earliest"`, `"latest"` or `"pending"`, as in the [default block parameter](#the-default-block-parameter).

```js
params: [


	'0xe8', // 232

]
```

##### Returns

`QUANTITY` - integer of the number of transactions in this block.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockTransactionCountByNumber","params":["0xe8"],"id":1}'

// Result
{


	"id":1,
	"jsonrpc": "2.0",
	"result": "0xa" // 10

}
```

***

## [GetCode](https://github.com/DSiSc/apigateway/tree/master/rpc/core/block.go?s=8801:8889#L338)
#### eth_getCode

Returns code at a given address.

##### Parameters

1. `DATA`, 20 Bytes - address.
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter).

```js
params: [


	'0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b',
	'0x2'  // 2

]
```

##### Returns

`DATA` - the code from the given address.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getCode","params":["0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b", "0x2"],"id":1}'

// Result
{


	"id":1,
	"jsonrpc": "2.0",
	"result": "0x600160008035811a818181146012578301005b601b6001356025565b8060005260206000f25b600060078202905091905056"

}
```

***

## [GetTransactionByBlockHashAndIndex](https://github.com/DSiSc/apigateway/tree/master/rpc/core/tx.go?s=15108:15214#L540)
Result see [eth_getTransactionByHash](#eth_gettransactionbyhash)
***

## [GetTransactionByBlockNumberAndIndex](https://github.com/DSiSc/apigateway/tree/master/rpc/core/tx.go?s=16209:16324#L578)
#### eth_getTransactionByBlockNumberAndIndex

Returns information about a transaction by block number and transaction index position.

##### Parameters

1. `QUANTITY|TAG` - a block number, or the string `"earliest"`, `"latest"` or `"pending"`, as in the [default block parameter](#the-default-block-parameter).
2. `QUANTITY` - the transaction index position.

```js
params: [


	'0x29c', // 668
	'0x0' // 0

]
```

##### Returns

See [eth_getTransactionByHash](#eth_gettransactionbyhash)

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionByBlockNumberAndIndex","params":["0x29c", "0x0"],"id":1}'
```

Result see [eth_getTransactionByHash](#eth_gettransactionbyhash)

***

## [GetTransactionByHash](https://github.com/DSiSc/apigateway/tree/master/rpc/core/tx.go?s=6487:6559#L211)
#### eth_getTransactionByHash

Returns the information about a transaction requested by transaction hash.

##### Parameters

1. `DATA`, 32 Bytes - hash of a transaction

```js
params: [


	"0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"

]
```

##### Returns

`Object` - A transaction object, or `null` when no transaction was found:

- `blockHash`: `DATA`, 32 Bytes - hash of the block where this transaction was in. `null` when its pending.
- `blockNumber`: `QUANTITY` - block number where this transaction was in. `null` when its pending.
- `from`: `DATA`, 20 Bytes - address of the sender.
- `gas`: `QUANTITY` - gas provided by the sender.
- `gasPrice`: `QUANTITY` - gas price provided by the sender in Wei.
- `hash`: `DATA`, 32 Bytes - hash of the transaction.
- `input`: `DATA` - the data send along with the transaction.
- `nonce`: `QUANTITY` - the number of transactions made by the sender prior to this one.
- `to`: `DATA`, 20 Bytes - address of the receiver. `null` when its a contract creation transaction.
- `transactionIndex`: `QUANTITY` - integer of the transactions index position in the block. `null` when its pending.
- `value`: `QUANTITY` - value transferred in Wei.
- `v`: `QUANTITY` - ECDSA recovery id
- `r`: `DATA`, 32 Bytes - ECDSA signature r
- `s`: `DATA`, 32 Bytes - ECDSA signature s

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionByHash","params":["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],"id":1}'

// Result
{


	"jsonrpc":"2.0",
	"id":1,
	"result":{
	  "blockHash":"0x1d59ff54b1eb26b013ce3cb5fc9dab3705b415a67127a003c3e61eb445bb8df2",
	  "blockNumber":"0x5daf3b", // 6139707
	  "from":"0xa7d9ddbe1f17865597fbd27ec712455208b6b76d",
	  "gas":"0xc350", // 50000
	  "gasPrice":"0x4a817c800", // 20000000000
	  "hash":"0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
	  "input":"0x68656c6c6f21",
	  "nonce":"0x15", // 21
	  "to":"0xf02c1c8e6114b1dbe8937a39260b5b0a374432bb",
	  "transactionIndex":"0x41", // 65
	  "value":"0xf3dbb76162000", // 4290000000000000
	  "v":"0x25", // 37
	  "r":"0x1b5e176d927f8e9ab405058b2d2457392da3e20f328b16ddabcebc33eaac5fea",
	  "s":"0x4ba69724e8f69de52f0125ad8b3c5c2cef33019bac3249e2c0a2192766d1721c"
	}

}
```
***

## [GetTransactionCount](https://github.com/DSiSc/apigateway/tree/master/rpc/core/block.go?s=10247:10348#L396)
#### eth_getTransactionCount

Returns the number of transactions *sent* from an address.

##### Parameters

1. `DATA`, 20 Bytes - address.
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter)

```js
params: [


	'0xc94770007dda54cF92009BFF0dE90c06F603a09f',
	'latest' // state at the latest block

]
```

##### Returns

`QUANTITY` - integer of the number of transactions send from this address.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionCount","params":["0xc94770007dda54cF92009BFF0dE90c06F603a09f","latest"],"id":1}'

// Result
{


	"id":1,
	"jsonrpc": "2.0",
	"result": "0x1" // 1

}
```

***

## [GetTransactionReceipt](https://github.com/DSiSc/apigateway/tree/master/rpc/core/tx.go?s=11549:11618#L396)
#### eth_getTransactionReceipt

Returns the receipt of a transaction by transaction hash.

**Note** That the receipt is not available for pending transactions.

##### Parameters

1. `DATA`, 32 Bytes - hash of a transaction

```js
params: [


	'0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238'

]
```

##### Returns

`Object` - A transaction receipt object, or `null` when no receipt was found:

- `transactionHash `: `DATA`, 32 Bytes - hash of the transaction.
- `transactionIndex`: `QUANTITY` - integer of the transactions index position in the block.
- `blockHash`: `DATA`, 32 Bytes - hash of the block where this transaction was in.
- `blockNumber`: `QUANTITY` - block number where this transaction was in.
- `from`: `DATA`, 20 Bytes - address of the sender.
- `to`: `DATA`, 20 Bytes - address of the receiver. null when its a contract creation transaction.
- `cumulativeGasUsed `: `QUANTITY ` - The total amount of gas used when this transaction was executed in the block.
- `gasUsed `: `QUANTITY ` - The amount of gas used by this specific transaction alone.
- `contractAddress `: `DATA`, 20 Bytes - The contract address created, if the transaction was a contract creation, otherwise `null`.
- `logs`: `Array` - Array of log objects, which this transaction generated.
- `logsBloom`: `DATA`, 256 Bytes - Bloom filter for light clients to quickly retrieve related logs.

It also returns _either_ :

- `root` : `DATA` 32 bytes of post-transaction stateroot (pre Byzantium)
- `status`: `QUANTITY` either `1` (success) or `0` (failure)

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionReceipt","params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],"id":1}'

// Result
{
"id":1,
"jsonrpc":"2.0",
"result": {


	   transactionHash: '0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238',
	   transactionIndex:  '0x1', // 1
	   blockNumber: '0xb', // 11
	   blockHash: '0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b',
	   cumulativeGasUsed: '0x33bc', // 13244
	   gasUsed: '0x4dc', // 1244
	   contractAddress: '0xb60e8dd61c5d32be8058bb8eb970870f07233155', // or null, if none was created
	   logs: [{
	       // logs as returned by getFilterLogs, etc.
	   }, ...],
	   logsBloom: "0x00...0", // 256 byte bloom filter
	   status: '0x1'
	}

}
```
***

## [RPCMarshalBlock](https://github.com/DSiSc/apigateway/tree/master/rpc/core/block.go?s=11269:11360#L435)
## [SendTransaction](https://github.com/DSiSc/apigateway/tree/master/rpc/core/tx.go?s=2549:2611#L74)
#### eth_sendTransaction

Creates new message call transaction or a contract creation, if the data field contains code.

##### Parameters

1. `Object` - The transaction object
- `from`: `DATA`, 20 Bytes - The address the transaction is send from.
- `to`: `DATA`, 20 Bytes - (optional when creating new contract) The address the transaction is directed to.
- `gas`: `QUANTITY`  - (optional, default: 90000) Integer of the gas provided for the transaction execution. It will return unused gas.
- `gasPrice`: `QUANTITY`  - (optional, default: To-Be-Determined) Integer of the gasPrice used for each paid gas
- `value`: `QUANTITY`  - (optional) Integer of the value sent with this transaction
- `data`: `DATA`  - The compiled code of a contract OR the hash of the invoked method signature and encoded parameters. For details see [Ethereum Contract ABI](<a href="https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI">https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI</a>)
- `nonce`: `QUANTITY`  - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.

```js
params: [{


	"from": "0xb60e8dd61c5d32be8058bb8eb970870f07233155",
	"to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567",
	"gas": "0x76c0", // 30400
	"gasPrice": "0x9184e72a000", // 10000000000000
	"value": "0x9184e72a", // 2441406250
	"data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"

}]
```

##### Returns

`DATA`, 32 Bytes - the transaction hash, or the zero hash if the transaction is not yet available.

Use [eth_getTransactionReceipt](#eth_gettransactionreceipt) to get the contract address, after the transaction was mined, when you created a contract.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_sendTransaction","params":[{see above}],"id":1}'

// Result
{


	"id":1,
	"jsonrpc": "2.0",
	"result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"

}
```

***

## [SetSwCh](https://github.com/DSiSc/apigateway/tree/master/rpc/core/tx.go?s=476:511#L23)
## [TypeConvert](https://github.com/DSiSc/apigateway/tree/master/rpc/core/block.go?s=10877:10917#L418)


---
