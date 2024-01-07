---
description: >-
  Subgraph component is to put pieces together, create & re-create wasm-hosts,
  process blocks and spit out results to Database.
---

# Subgraph

Subgraph component will use Datasource info (abis, wasm-module) to create Wasm-Hosts -  which is used to run the Wasm-Module compiled by TheGraph' SDK - and pass blocks data to such wasm-hosts for indexing.

### Creating & re-creating WASM hosts

A WASM host is a sand-boxed environment to run WebAssembly script.&#x20;

In the case of Datafast Runtime, it runs the AssemblyScript-compiled WASM module. Datafast Runtime use [wasmer](https://docs.rs/wasmer/latest/wasmer/index.html) to build the wasm-host. (TheGraph's graph-node uses [wasmtime](https://docs.rs/wasmtime/latest/wasmtime/))

WASM host has its own memory pool, which is typically 4Gb limit.

Because of how TheGraph's SDK compiles subgraph's scripts, the WASM host has no garbage collector. Therefore, as the indexing progresses, WASM host's memory will keep growing indefinitely until it crashes due to insufficient memory.

Due to such behavior, wasm-host's memory  usage must be continuously tracked. [When a wasm-host's memory is about to peak,](https://github.com/datafast-network/datafast-runtime/blob/main/src/components/subgraph/datasource\_wasm\_instance.rs#L116) such wasm-host must be [dropped and a new one should be created](https://github.com/datafast-network/datafast-runtime/blob/main/src/components/subgraph/mod.rs#L54) to handle the later data.

The current logic for handling is, when the memory usage of a wasm-host is over 50%, it will be dropped and a new one will be created. The rationale behind this number (50%) is as following:

* Re-creating a brand new wasm-host on every new block before processing is very time-consuming (20\~30ms) - which will make the runtime become very slow when processing multi-million of blocks
* Re-creating a new wasm-host when the memory usage is too close to limit (eg 90%-ish) will make the host-dropping and memory re-collecting very slow as well - up to 200\~300ms.
