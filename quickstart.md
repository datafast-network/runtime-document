---
description: How to quickly run a demo with Datafast Runtime
---

# 🚀 Quickstart

## Prerequisites

Before running Datafast-Runtime, you have to prepare…

* a S3-Compatible object storage (used as data-warehouse) which contains blockchain data, using [Delta Ingestor.](https://www.notion.so/Delta-Ingestor-246d8ff3c77f4e0997a4ad5f81ecc9ab?pvs=21) The number of blocks should be enough to index your subgraph
* A subgraph with at least **version ≥ 0.0.5** correctly codegen-ed & built. Refer to [https://thegraph.com/docs/en/developing/creating-a-subgraph/](https://thegraph.com/docs/en/developing/creating-a-subgraph/)
* A working Ethereum JSON-RPC, you can use a premium node or public node in [https://chainlist.org/chain/1](https://chainlist.org/chain/1)
* A database that Datafast-Runtime can use to store output data (currently only MongoDB supported)
* Rust toolchain (cargo, stuffs...)

## Running

Clone the repository

```
$ git clone git@github.com:datafast-network/datafast-runtime.git
```

Export required environment variables, including Object Storage host & credential, location of data store etc

```bash
export AWS_ENDPOINT=your-s3-endpoint
export AWS_REGION=your-region
export AWS_S3_ALLOW_UNSAFE_RENAME=true
export AWS_SECRET_ACCESS_KEY=your-s3-secret-key
export AWS_ACCESS_KEY_ID=your-s3-access-key
export AWS_ALLOW_HTTP=true
export AWS_FORCE_PATH_STYLE=true
export TABLE_PATH=your-path-to-bucket-or-director-that-contain-block-data
export RUST_LOG=info,deltalake=off
```

Create config file in the repository folder with name as `config.toml`

```toml
chain = "ethereum"
subgraph_name = "my-subgraph"
subgraph_id = "1234"
subgraph_dir = "/local-path-to-your-subgraph-dir/build"
reorg_threshold = 500
rpc_endpoint = "wss://ethereum-json-rpc-endpoint"

[source.delta]
table_path = "s3://ethereum/blocks-data/"
query_step = 20000

[database.mongo]
uri = "mongodb://root:example@localhost:27017"
database = "db0"

[valve]
allowed_lag = 10
wait_time = 10
```

Run the runtime with...

```bash
$ cargo run --release
```
