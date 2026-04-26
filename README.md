# Arbitration Contract Verification

**Contract (instance 1):** `0xe881af13bf55c97562fe8d2da2f6ea8e3ff66f98`  
**Contract (instance 2):** `0x7e2d0fe0ffdd78c264f8d40d19acb7d04390c6e8`  
**Deployer:** `0x1db3439a222c519ab44bb1144fc28167b4fa6ee6` (Vitalik Buterin early dev address)  
**Block (instance 1):** 303,316 | **Block (instance 2):** 318,029  
**Date (instance 1):** September 28, 2015 | **Date (instance 2):** October 1, 2015  
**Language:** Serpent  
**Status:** ✅ Exact bytecode match (byte-for-byte, both instances verified separately)

## What This Is

The escrow/arbitration contract from Vitalik Buterin's experimental Ethereum arbitration dapp (Sep–Oct 2015). Parties create escrow contracts, designate arbiters to vote on disputed outcomes, and funds are released based on arbiter votes. This is one of the earliest decentralized dispute resolution contracts on Ethereum.

## Source

Both instances compile to 2,544 runtime bytes from `arbitration.se` using Serpent
[`e5a5f875`](https://github.com/ethereum/serpent/commit/e5a5f875f347d45b05368ca257b296313b643977)
(Sep 26, 2015). They differ in one line: the argument order of the `ArbiterNotification` log call.

**Instance 1 — `arbitration.se`** (corrected argument order)
- Source from [ethereum/dapp-bin](https://github.com/ethereum/dapp-bin), base commit
  [`08fe3e5b`](https://github.com/ethereum/dapp-bin/commit/08fe3e5b18a9cb9bf5e2c8ed10478ddb4f3906b1),
  with one fix: `log(type=ArbiterNotification, arbiters[i], id)` (Vitalik's corrected order)

**Instance 2 — `arbitration_v2.se`** (original dapp-bin argument order)
- Same source as above, but using the original dapp-bin line:
  `log(type=ArbiterNotification, id, arbiters[i])`
- Deployed 3 days later; Vitalik used the unmodified dapp-bin version

The argument order swap produces exactly 12 bytes of difference (at runtime offset 0x0119–0x0124),
changing the order in which the two event topics are pushed onto the EVM stack before the LOG2 opcode.

## Compiler

**Language:** Serpent  
**Repository:** [ethereum/serpent](https://github.com/ethereum/serpent)  
**Commit:** [`e5a5f875`](https://github.com/ethereum/serpent/commit/e5a5f875f347d45b05368ca257b296313b643977)  
**Date:** September 26, 2015

## Key Contract Features

- **mk_contract()**: Create an escrow with two parties, up to 10,000 arbiters, and a fee
- **vote(id, voteForA)**: Arbiters vote; >50% majority pays out to winner
- **get_contract_value/recipients/arbiters**: Read contract state
- **Short-circuit**: Parties can instantly transfer to the other by voting voteForA=0 (if recipientA) or voteForA=1 (if recipientB)
- **Cleanup**: On resolution, all storage zeroed out to save gas

## Proof

| Field | Instance 1 | Instance 2 |
|-------|-----------|-----------|
| On-chain address | `0xe881af13bf55c97562fe8d2da2f6ea8e3ff66f98` | `0x7e2d0fe0ffdd78c264f8d40d19acb7d04390c6e8` |
| Source file | `arbitration.se` | `arbitration_v2.se` |
| Key difference | `log(..., arbiters[i], id)` | `log(..., id, arbiters[i])` |
| Runtime size | 2,544 bytes | 2,544 bytes |
| Byte differences | **0** (exact match) | **0** (exact match) |
