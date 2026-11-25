# Bitcoin Puzzle Transaction — Tracker & Reference

An ongoing tracker and knowledge base for the **Bitcoin Puzzle Transaction**, also known as the **1000 BTC Challenge** — one of the most fascinating cryptographic puzzles in Bitcoin history.

## What Is the Bitcoin Puzzle?

In January 2015, an anonymous user created a [single transaction](https://www.blockchain.com/explorer/transactions/btc/08389f34c98c606322740c0be6a7125d9860bb8d5cb182c02f98461e5fa6cd15) sending Bitcoin to **256 addresses**, each locked by a private key with an incrementally increasing bit length. The amounts matched the key size: puzzle #1 received 0.001 BTC, puzzle #2 received 0.002 BTC, and so on up to puzzle #256 at 0.256 BTC — totaling ~32.9 BTC.

The private keys are random values within constrained ranges. Puzzle #N has a key between 2^(N-1) and 2^N, meaning each puzzle is **exactly twice as hard** as the previous one.

The creator later described it on BitcoinTalk:

> *"There is no pattern. It is just consecutive keys from a deterministic wallet (masked with leading 000...0001 to set difficulty). It is simply a crude measuring instrument, of the cracking strength of the community."*

## Timeline of Key Events

**2015-01-15** — Original transaction created in block [#339085](https://www.blockchain.com/explorer/transactions/btc/08389f34c98c606322740c0be6a7125d9860bb8d5cb182c02f98461e5fa6cd15). 256 addresses funded with ~32.9 BTC total. Puzzles #1 through ~50 solved almost immediately.

**2017-07-11** — The creator moved funds from addresses #161–256 (unsolvable in practice) to the lower-range addresses #53–160, effectively **increasing the prize pool by 10x**.

**2019-05-31** — Creator sends 1,000-satoshi transactions from addresses #65, #70, #75, #80… #160 (every 5th key). This **exposes the public keys** for those addresses, enabling the use of the Pollard's kangaroo algorithm — reducing the search space from O(2^N) to O(2^(N/2)).

**2023-04-16** — Creator increases unsolved puzzle prizes **by another 10x**, bringing the total pool to approximately **~1,000 BTC**. Puzzle #66 is now worth 6.6 BTC, #67 is 6.7 BTC, all the way up to #160 at 16 BTC.

**2024-09-12** — **Puzzle #66 solved** (6.6 BTC, ~$400K). The solver broadcast the transaction publicly, and it was **front-run in the mempool** — the original transaction was replaced, and most of the prize was taken by a different address.

**2025-02-21** — **Puzzle #67 solved** (6.7 BTC). Lessons learned from #66: the solver bypassed the public mempool entirely to avoid interception.

**2025-04-07** — **Puzzle #68 solved** (6.8 BTC). Also mined bypassing the public mempool.

**2025-04-30** — **Puzzle #69 solved** (6.9 BTC). The key was found near the very beginning of the search range (0.72%). The solver broadcast publicly, and it was **stolen again** — bots extracted the public key from the mempool, computed the private key, and replaced the transaction multiple times.

## Current State

**82 puzzles solved** out of 160. All keys from #1–#70, plus every 5th key up to #130 (#75, #80, #85, #90, #95, #100, #105, #110, #115, #120, #125, #130) have been cracked.

**Next targets:**

- **Puzzle #71** — 71-bit key space, brute-force only. The easiest remaining puzzle. Reward: 7.1 BTC. Estimated: ~33.55 million search ranges. A single RTX 4090 at ~1.5 billion keys/second would take roughly **25,000 years** — but distributed GPU pools are actively working on it.
- **Puzzle #135** — Public key is exposed, making it solvable with the Pollard's kangaroo algorithm. Reward: 13.5 BTC.

**Remaining prize pool:** ~900+ BTC across unsolved puzzles #71–#160 (excluding every 5th key already cracked up to #130).

## How Puzzles Are Solved

**Brute-force (puzzles without exposed public keys):** Iterate through all possible private keys in the range 2^(N-1) to 2^N. For each key, derive the Bitcoin address and check if it matches the target. Difficulty doubles with every puzzle.

**Pollard's kangaroo algorithm (every 5th puzzle with exposed public keys):** The creator's 1,000-satoshi transactions exposed public keys for #65, #70, #75… etc. This allows solving the Elliptic Curve Discrete Logarithm Problem (ECDLP) directly, reducing complexity from O(2^N) to O(2^(N/2)) — a massive speedup.

**The mempool problem:** When a solver broadcasts a spending transaction, the private key becomes visible in the mempool. Bots can extract it and submit a competing transaction with a higher fee. This has led to prize theft on puzzles #66 and #69. Solvers now use private mining services (like MARA Slipstream) to bypass the public mempool.

## Tools & Software

| Tool | Type | Description |
|---|---|---|
| [BitCrack](https://github.com/brichard19/BitCrack) | GPU brute-force | CUDA/OpenCL key space search |
| [Kangaroo](https://github.com/JeanLucPons/Kangaroo) | GPU ECDLP | Pollard's kangaroo for puzzles with exposed public keys |
| [Keyhunt](https://github.com/albertobsd/keyhunt) | CPU | BSGS and brute-force modes |
| [RCKangaroo](https://github.com/RetiredC/RCKangaroo) | GPU ECDLP | SOTA Kangaroo with symmetry (K=1.15). ~8 GKeys/s on RTX 4090. Used to solve puzzles #125, #130, and others |
| [Keyhunt-CUDA](https://github.com/WanderingPhilosopher/keyhunt-cuda) | GPU | CUDA-accelerated version of Keyhunt |

## Community & Discussion

- [BitcoinTalk — Original thread](https://bitcointalk.org/index.php?topic=1306983.0) — The primary community discussion, running since 2015. Thousands of posts covering tools, strategies, solved keys, and analysis.
- [BitcoinTalk — Challenge thread (updated)](https://bitcointalk.org/index.php?topic=5218972.0) — The "~1000 BTC total bounty" thread with current status updates.
- [privatekeys.pw/puzzles/bitcoin-puzzle-tx](https://privatekeys.pw/puzzles/bitcoin-puzzle-tx) — Comprehensive tracker with all addresses, solved keys, and transaction links.
- [btcpuzzle.info](https://btcpuzzle.info/) — Puzzle list, progress calculator, and pool-based searching.
- [Written in Bitcoin — puzzle status](https://writteninbitcoin.com/en/puzzles/bitcoin-puzzle-transaction) — All 160 prizes with key range, address and amount, and a live on-chain check whether each prize is still unspent, read from a full node. No key search.
- [Hacker News discussion (Puzzle #66)](https://news.ycombinator.com/item?id=41547395) — Technical discussion around the September 2024 solve.
- [OEIS A369920](https://oeis.org/A369920) — The solved private keys are registered as a sequence in the Online Encyclopedia of Integer Sequences.

## Why This Matters

The Bitcoin Puzzle is more than a treasure hunt. It serves as:

- **A real-time benchmark** of the community's key-cracking capability against elliptic curve cryptography
- **A canary for quantum computing threats** — when higher puzzles start falling faster than expected, it signals advancing threats to Bitcoin's cryptographic assumptions
- **A practical lesson in operational security** — the mempool front-running incidents on puzzles #66 and #69 demonstrate real risks in transaction broadcasting

## Repository Contents

- `BTC-Solved-Unsolved.txt` — Current state of all 160 puzzles (solved/unsolved, key values where known)

## Contributing

If you have corrections, additional historical events, or links to resources, feel free to open an issue or PR.

## License

GPL-3.0
