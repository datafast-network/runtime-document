---
description: Interacting directly with blockchain JSON-RPC apis
---

# JSON-RPC Client

[JSON-RPC Client ](https://github.com/datafast-network/datafast-runtime/blob/main/src/rpc\_client/mod.rs#L65)allows Runtime to interact with blockchain JSON-RPC api through subgraph's code.

Currently it only supports Evm-blockchain, meaning Eth-Calls, but can be easily extended to work with other chains.

JSON-RPC Client comes with its own rpc-call cache mechanism.

There are 2 levels of caching:

* block-level caching: rpc-calls that can be cached at block level - meaning if the calls are repeated during the processing of the same block, their results will be reused.
* chain-level caching: rpc-calls that can be cached at chain level - meaning the rpc calls at different blocks still result in the same values. These values must be manually defined. Examples for chain-level caching calls are typically `ERC20-Symbols, ERC20-Token-Name`

