# Arbitration Contract Verification

**Contract (instance 1):** `0xe881af13bf55c97562fe8d2da2f6ea8e3ff66f98`  
**Contract (instance 2):** `0x7e2d0fe0ffdd78c264f8d40d19acb7d04390c6e8`  
**Deployer:** `0x1db3439a222c519ab44bb1144fc28167b4fa6ee6` (Vitalik Buterin early dev address)  
**Block (instance 1):** 303,316 | **Block (instance 2):** 318,029  
**Date (instance 1):** September 28, 2015 | **Date (instance 2):** October 6, 2015  
**Language:** Serpent  
**Status:** ✅ Exact bytecode match (byte-for-byte, both instances)

## What This Is

The escrow/arbitration contract from Vitalik Buterin's experimental Ethereum arbitration dapp (Sep 2015). Parties create escrow contracts, designate arbiters to vote on disputed outcomes, and funds are released based on arbiter votes. This is one of the earliest decentralized dispute resolution contracts on Ethereum.

## Source

**File:** `arbitration.se`  
**Repository:** [ethereum/dapp-bin](https://github.com/ethereum/dapp-bin)  
**Base commit:** [`08fe3e5b`](https://github.com/ethereum/dapp-bin/commit/08fe3e5b18a9cb9bf5e2c8ed10478ddb4f3906b1)  
**Note:** The `arbitration.se` in this repo has one line corrected from dapp-bin's 08fe3e5b: the `ArbiterNotification` log call had its indexed arguments in reversed order (`arbiters[i], id` instead of `id, arbiters[i]`). Vitalik fixed this in a later commit but deployed with the original order. The correction matches the on-chain bytecode exactly.

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
- **Cleanup**: On resolution, all storage zeroed out to save gas (good citizen of the blockchain!)

## Proof

| Field | Value |
|-------|-------|
| On-chain address 1 | `0xe881af13bf55c97562fe8d2da2f6ea8e3ff66f98` |
| Creation tx 1 | `0xd9a870d9264ead47477f8ed713a49edd3c461e5f50a6152772396578df76d739` |
| On-chain address 2 | `0x7e2d0fe0ffdd78c264f8d40d19acb7d04390c6e8` |
| Runtime size | 2,544 bytes |
| Differences | **0** (exact match) |
