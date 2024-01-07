---
description: >-
  Datafast Runtime is a high-performance subgraph processing runtime which is
  written from scratch and designed to handle subgraphs with unparalleled speed
  & storage-efficiency
---

# 😁 Introduction

**Code repository at** [**https://github.com/vutran1710/datafast-runtime**](https://github.com/datafast-network/datafast-runtime)

**Document repository at** [**https://github.com/vutran1710/datafast-runtime-document**](https://github.com/datafast-network/runtime-document)

<figure><img src=".gitbook/assets/hardbed-logo.png" alt="" width="563"><figcaption></figcaption></figure>

## What does Datafast Runtime do?

❤️  Performance-focus - it is built to prioritize indexing speed

❤️  Blockchain data reusability - block data is stored in Object Storage for reusability, eliminating cost of calling JSON-RPC over and over again for the same data

❤️  Blockchain data is smartly organized & compressed using Databricks' DeltaLake technology plus Protobuf on top for storage-efficiency, very fast serialization, very fast to ingest and unlimited scalability

❤️  Indexed data is stored in NoSQL database to reduce database cost yet still fast in retrieval

❤️  Allowing user to specify a limit time to keep output data in store, like last 6 months or last year - help reduce data storage cost by remove outdated data

❤️  Graceful reorg-handling

❤️  Extensible - as Datafast code base is way simpler and extensible than graph-node, allowing user to freely implement more features without crying for help

❤️  No need IPFS!

❤️  No GAS required for indexing!



## What Datafast Runtime cannot do...

❌  Datafast Runtime is built to focus on the indexing-job. It does not come with a GraphQL query layer.



## 🚧  Development Status

Datafast is built for educational & sharing purpose & not ready for production - since there are plenty of features that needs implementing & issues need addressing.&#x20;

Still, Datafast-Runtime can be used as a ground-work to build in-house blockchain indexing solution to replace GraphNode / TheGraph



## 🔰  Who is this runtime for?

* Companies who want to host their own subgraph data
* Companies who want to host their own subgraph data with efficient storage cost, with their own solution
* Developers who want to play around with their own version of Subgraph Indexing Engine
* Developers who want to play around with WebAssembly technology and build crazier stuffs with it.
