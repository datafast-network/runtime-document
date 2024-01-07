---
description: >-
  Datafast Runtime is a high-performance processing subgraph-runtime which is
  written from scratch and designed to handle subgraphs with unparalleled speed
  & storage-efficiency
---

# 😁 Introduction

{% embed url="https://github.com/datafast-network/datafast-runtime" %}

> **Datafast Runtime is a high-performance processing subgraph-runtime which is written from scratch and designed to handle subgraphs with unparalleled speed & NoSQL storage. With this runtime, your data processing tasks will be faster and more storage-cost efficient than the traditional GraphNode from TheGraph**

<figure><img src=".gitbook/assets/hardbed-logo.png" alt="" width="563"><figcaption></figcaption></figure>

## What Datafast Runtime can do better than GraphNode/TheGraph?

❤️  Performance-focus - it is built to prioritize indexing speed

❤️  Blockchain data reusability - block data is stored in Object Storage for reusability, eliminating cost of calling JSON-RPC over and over again for the same data

❤️  Blockchain data is smartly organized & compressed using Databricks' DeltaLake technology plus Protobuf on top for storage-efficiency, very fast serialization, very fast to ingest and unlimited scalability

❤️  Output, indexed data is stored in NoSQL database to reduce database cost yet still fast in retrieval

❤️  Allowing user to specify a limit time to keep output data in store, like last 6 months or last year - help reduce data storage cost by remove outdated data

❤️  Graceful reorg-handling

❤️  Extensible - as Datafast code base is way simpler and extensible than graph-node, allowing user to freely implement more features without crying for help



## What Datafast Runtime cannot do compared to GraphNode / TheGraph

❌  Datafast Runtime is built to focus on the indexing-job. It does not and is not going to ever come with a GraphQL query layer.

