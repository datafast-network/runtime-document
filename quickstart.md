---
description: Quickly run a demo with Datafast Runtime
---

# 🚀 Quickstart

## Prerequisites

Before running Datafast-Runtime using this Quickstart''s tutorial, you have to prepare yourself…

*   A subgraph with at least **version ≥ 0.0.5** correctly codegen-ed & built. Refer to [https://thegraph.com/docs/en/developing/creating-a-subgraph/](https://thegraph.com/docs/en/developing/creating-a-subgraph/).&#x20;

    _If you don't have a subgraph already, you can use our example Uniswap-V3 subgraph at_ [_https://github.com/datafast-network/subgraph-testing/tree/main/packages/uniswap-v3_](https://github.com/datafast-network/subgraph-testing/tree/main/packages/uniswap-v3)

    _To use the example Uniswap-V3 subgraph, you need either npm, yarn or pnpm._

    _Clone the repo and build the code with `npm run build` or `yarn build` or `pnpm build`_
* Rust toolchain (cargo, stuffs...) to compile the code to the binary, executable file.

## Running

Clone the repository

```
$ git clone git@github.com:datafast-network/datafast-runtime.git
```

Enable the quickstart script

```
$ chmod +x quickstart.sh
```

Run the script with env `DFR_SUBGRAPH_DIR` exported being the <mark style="color:orange;">**absolute path to your subgraph's build-dir**</mark> _(when using the example subgraph, path should be something like `some-path/uniswap-v3/build`)_

```
$ DFR_SUBGRAPH_DIR=/path/to/your/subgraph/build/dir ./quickstart.sh
```

When running, the script will do the following:

* export required environment variables, including...
  * &#x20;credentials to access the demo blockchain data store we prepared
  * `CONFIG` env to the `quickstart_config.toml`
* docker compose up the demo database (MongoDB & MongoExpress)
* run the runtime with `cargo run --release`

<figure><img src=".gitbook/assets/Screenshot 2024-01-07 at 20.16.19.png" alt="" width="563"><figcaption></figcaption></figure>
