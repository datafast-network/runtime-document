---
description: >-
  https://github.com/datafast-network/datafast-runtime/blob/main/src/components/block_source/mod.rs
---

# BlockSource

BlockSource polls the block-store for archived blockchain data & also new data.&#x20;

There can be multiple variants of BlockSource. Currently the only variant implemented is DeltaLake which is the product of Delta-Ingestor

## [DeltaLake block source by DeltaIngestor](https://github.com/vutran1710/delta-ingestor)

<figure><img src="../.gitbook/assets/68747470733a2f2f646f63732e64656c74612e696f2f6c61746573742f5f7374617469632f64656c74612d6c616b652d77686974652e706e67.png" alt="" width="163"><figcaption></figcaption></figure>

The data in DeltaLake block source produced by DeltaIngestor comes in DeltaLake format - meaning it is typically a LakeHouse data store.

DeltaLake is a new file format developed by Databricks, which basically is based from Parquet file format. This file-based data store helps reduce storage cost compared to Postgres and at the same time eliminate JSON-RPC repetitive calls for the same data.

This data format is a combination of both DeltaLake format & Protobuf, with the following schema definition:

| Column names     | Column types | Description                                                                                                                                                                                                                                                                |
| ---------------- | ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| block\_number    | u64 / long   | number of block                                                                                                                                                                                                                                                            |
| block\_partition | u64 / long   | <p>partition key for delta-lake, calculated by <br><code>block_number % A_PARITION_MOD_VALUE</code></p>                                                                                                                                                                    |
| hash             | string       | block's hash                                                                                                                                                                                                                                                               |
| parent\_hash     | string       | block's parent-hash. The hash & parent-hash is used when handling duplicate data / reorg data                                                                                                                                                                              |
| block\_data      | bytes        | Protobuf byte format. In case of ethereum, its definition can be found in [https://github.com/datafast-network/delta-ingestor/blob/main/ingestor/src/proto/ethereum.proto](https://github.com/datafast-network/delta-ingestor/blob/main/ingestor/src/proto/ethereum.proto) |
| created\_at      | u64 / long   | The timestamp of when the block is written to the delta-lake - not actual block-mining timestamp. This value is to help with handling reorg & duplicate data during the Ingestor's running process                                                                         |

The `block_data` column is Protobuf's byte data. By utilizing protobuf rather than human-readable data, it ensures the fast serialization / deserialization when ingesting or processing the data in the delta lake.

The deltalake block source uses [**datafusion**](https://arrow.apache.org/datafusion/) SQL API to query blocks from [DeltaLake](https://delta.io/) for processing in an infinite polling loop, which can be described with the following pseudo-code:

```python
block_start = 0
number_of_block = 20_000

while true:
  blocks = query_delta("SELECT block_data FROM blocks WHERE block_number >= {block_start} AND block_number < {block_start + number_of_block}")
  block_messages = serialize_into_message(blocks)
  channel_sender.send(block_messages)
  block_start += number_of_block
```

The downloaded data is read & parsed into the runtime's compatible data format. This work is done in parallel-fashion using [Rayon](https://docs.rs/rayon/latest/rayon/index.html)&#x20;

When serializing is finished, data is wrapped in a Message struct and forwarded to the main-flow of the runtime through a [kanal](https://github.com/fereidani/kanal) channel.

#### Configuration

DeltaLake block source's configuration requires 2 values: the table path & the query step

* Table path is the S3 path to the data directory in the Object Storage being in use.
* The query-step is the number of blocks the source client would download for each round of query

The configuration is defined in the `config.toml` in the following format

```toml
[source.delta]
table_path = "s3://ethereum/blocks_01/"
query_step = 20000
```
