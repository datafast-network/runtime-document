---
description: >-
  https://github.com/datafast-network/datafast-runtime/blob/main/src/components/inspector.rs
---

# BlockInspector

The role of BlockInspector is to ensure:

1. Data Integrity: only valid blocks are subjected for processing
2. Block Continuity: blocks must be processed sequentially and no blocks are missed

<figure><img src="../.gitbook/assets/77-779282_detective-royalty-free-vector-clip-art-illustration-secret.png" alt=""><figcaption></figcaption></figure>

To check for block validity, BlockInspector reads `subgraph.yaml` to determine the earliest start block of the subgraph and also reads from Database to know the current indexing progress.&#x20;

Incoming blocks must be sequential most of the time and block numbers must be correct based on the current indexing progress.

BlockInspector silently reject invalid blocks, or panic if incoming blocks are not in proper order.

In case of Ethereum indexing, BlockInspector only validate block by checking block numbers & block hashes - but when implementing other chains, there may be more values needed to be taken into account for data integrity confirmation.

```rust
match inspector.check_block(block) {
    BlockInspectionResult::UnexpectedBlock
    | BlockInspectionResult::UnrecognizedBlock => {
        panic!("Bad block data from source");
    }
    BlockInspectionResult::BlockAlreadyProcessed
    | BlockInspectionResult::MaybeReorg => {
        continue;
    }
    BlockInspectionResult::ForkBlock => {
        database.revert_from_block(block.number).await?;
    }
    BlockInspectionResult::OkToProceed => (),
};
```

BlockInspector does not need to be configured.
