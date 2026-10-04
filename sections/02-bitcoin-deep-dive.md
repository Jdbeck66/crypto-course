# Section 2: Bitcoin Deep Dive - Technical Architecture & Economics

## Table of Contents

- [2.1 The Bitcoin Network Architecture](#21-the-bitcoin-network-architecture)
- [2.2 Blockchain Data Structure](#22-blockchain-data-structure)
- [2.3 Transactions: The UTXO Model](#23-transactions-the-utxo-model)
- [2.4 Mining and Proof-of-Work Consensus](#24-mining-and-proof-of-work-consensus)
- [2.5 Bitcoin's Economic Model](#25-bitcoins-economic-model)
- [2.6 Bitcoin's Scripting Language](#26-bitcoins-scripting-language)
- [2.7 Network Security and Attack Vectors](#27-network-security-and-attack-vectors)
- [2.8 Scalability Challenges and Solutions](#28-scalability-challenges-and-solutions)
- [2.9 Bitcoin Use Cases and Narratives](#29-bitcoin-use-cases-and-narratives)
- [Key Takeaways](#key-takeaways)
- [Further Reading](#further-reading)
- [Computational Exercises](#computational-exercises)

---

## 2.1 The Bitcoin Network Architecture

### 2.1.1 Peer-to-Peer Network

> **Definition: Peer-to-Peer (P2P) Network**
>
> A peer-to-peer network is a distributed network architecture where each participant (node) acts as both a client and a server, communicating directly with other nodes without relying on a central server. In Bitcoin, every node can send, receive, and validate transactions and blocks independently.

The Bitcoin network is a flat, decentralized P2P network with no hierarchy or special nodes (at the protocol level). Any computer running the Bitcoin software can join the network, and all nodes are considered equal.

**Node discovery and connection:**
1. A new node connects to a set of "seed nodes" hardcoded in the Bitcoin software
2. The node requests addresses of other peers from its initial connections
3. The node establishes connections with multiple peers (typically 8 outbound, up to 125 total)
4. The node begins downloading and validating the entire blockchain history

### 2.1.2 Types of Nodes

While all nodes are equal at the protocol level, different implementations serve different purposes:

**Full Nodes:**
- Store the complete blockchain (~600 GB as of 2025)
- Independently validate every transaction and block against all consensus rules
- Do not trust any other node — they verify everything themselves
- Enforce the network's rules; invalid blocks are rejected regardless of who mined them

**Archival Nodes:**
- Full nodes that also maintain a complete index of all transactions
- Enable querying any historical transaction or address balance
- Require additional storage beyond the base blockchain

**Pruned Nodes:**
- Full nodes that validate all blocks but discard old block data after verification
- Retain only recent blocks and the current unspent transaction output (UTXO) set
- Reduce storage requirements (can run with as little as ~10 GB)
- Still provide the same security guarantees during validation

**Mining Nodes:**
- Full nodes that also perform proof-of-work computation
- Construct candidate blocks from the mempool
- Compete to find valid block hashes
- Typically connect to mining pools for consistent revenue

**Light Nodes (SPV Clients):**

> **Definition: Simplified Payment Verification (SPV)**
>
> SPV is a method described in the Bitcoin whitepaper that allows a client to verify that a transaction is included in a block without downloading the full block. SPV clients download only block headers (~80 bytes each) and use Merkle proofs to verify transaction inclusion. They trust that the longest chain represents valid transactions but do not independently validate all transactions.

- Download only block headers (not full blocks)
- Verify transactions using Merkle proofs
- Trust that the chain with the most proof-of-work contains valid transactions
- Used by most mobile wallets
- Tradeoff: reduced security for dramatically lower resource requirements

**Source:** Antonopoulos, A. (2017). Mastering Bitcoin, 2nd Edition. Chapter 8: The Bitcoin Network. https://github.com/bitcoinbook/bitcoinbook

### 2.1.3 The Mempool

> **Definition: Mempool (Memory Pool)**
>
> The mempool is a node's holding area for valid but unconfirmed transactions. When a user broadcasts a transaction, it propagates across the network and sits in each node's mempool until a miner includes it in a block. Each node maintains its own mempool independently — there is no single, global mempool. Transactions with higher fees are typically prioritized by miners.

When a user sends a Bitcoin transaction:
1. The transaction is broadcast to connected peers
2. Each peer validates the transaction (checks signatures, confirms UTXOs exist, verifies no double-spend)
3. Valid transactions are added to the node's mempool and relayed to other peers
4. Miners select transactions from their mempool to include in the next block
5. Once included in a block and confirmed, the transaction is removed from the mempool

The mempool acts as a fee market: when demand for block space exceeds supply, users compete by offering higher transaction fees. During periods of high activity, the mempool can grow to contain hundreds of thousands of unconfirmed transactions.

---

## 2.2 Blockchain Data Structure

### 2.2.1 Block Structure

A Bitcoin block consists of two parts: the block header and the list of transactions.

**Block Header (80 bytes):**

| Field | Size | Description |
|-------|------|-------------|
| Version | 4 bytes | Block version number (indicates which rules the block follows) |
| Previous Block Hash | 32 bytes | Double Secure Hash Algorithm 256 (SHA-256d) hash of the previous block's header |
| Merkle Root | 32 bytes | Hash of the root of the Merkle tree of all transactions in the block |
| Timestamp | 4 bytes | Unix timestamp (seconds since Jan 1, 1970) |
| Difficulty Target | 4 bytes | Compact representation of the target threshold for valid proof-of-work |
| Nonce | 4 bytes | Counter used in mining; miners iterate this value to find a valid hash |

**Key properties of the block header:**
- The previous block hash creates the "chain" — each block is cryptographically linked to its predecessor
- Changing any transaction in a block changes the Merkle root, which changes the block hash, which invalidates all subsequent blocks
- This cascading invalidation is what makes the blockchain tamper-resistant

**Block body:**
- Contains the list of transactions
- The first transaction is always the coinbase transaction (mining reward)
- Current block size limit: 4 MB (measured in "weight units" since the Segregated Witness (SegWit) upgrade)
- Typical block contains 2,000-4,000 transactions

### 2.2.2 Merkle Trees in Bitcoin

> **Definition: Merkle Tree**
>
> A Merkle tree (also called a hash tree) is a binary tree data structure where every leaf node contains the hash of a data block, and every non-leaf node contains the hash of its two children. The single hash at the top is called the Merkle root. Merkle trees allow efficient and secure verification that a specific piece of data is part of a larger dataset without needing the entire dataset.

Bitcoin uses Merkle trees to efficiently summarize all transactions in a block:

```
                    Merkle Root
                   /            \
              H(AB)              H(CD)
             /     \            /     \
         H(A)      H(B)    H(C)      H(D)
          |         |        |         |
        Tx A      Tx B     Tx C      Tx D
```

Where H() represents double SHA-256 hashing.

**Construction process:**
1. Hash each transaction with double SHA-256 to create leaf nodes
2. Pair adjacent hashes and concatenate them
3. Hash each pair to create the parent node
4. If there is an odd number of nodes, duplicate the last one
5. Repeat until a single hash remains — the Merkle root

**Why Merkle trees matter:**
- **Efficient verification:** A Merkle proof for one transaction in a block with 4,000 transactions requires only ~12 hashes (log2(4000) ≈ 12), not all 4,000 transaction hashes
- **SPV support:** Light clients can verify transaction inclusion with just block headers and a Merkle proof
- **Tamper detection:** Changing any transaction changes its hash, which propagates up through the tree, changing the Merkle root and therefore the block header hash

> **Notebook Reference:** See `notebooks/01-cryptographic-primitives.ipynb`, Part 4: Merkle Trees for a hands-on implementation of Merkle tree construction and proof verification.

### 2.2.3 Chain of Blocks

The blockchain is a singly-linked list of blocks, with each block containing the hash of the previous block's header:

```
Block 0          Block 1          Block 2
(Genesis)
+-----------+    +-----------+    +-----------+
| Prev: 0   |<---| Prev: H0  |<---| Prev: H1  |
| Merkle: M0|    | Merkle: M1|    | Merkle: M2|
| Nonce: N0 |    | Nonce: N1 |    | Nonce: N2 |
| Time: T0  |    | Time: T1  |    | Time: T2  |
+-----------+    +-----------+    +-----------+
| Tx0       |    | Tx0, Tx1  |    | Tx0...TxN |
| (coinbase)|    | ...TxN    |    |           |
+-----------+    +-----------+    +-----------+
```

**Immutability through chaining:**
If an attacker modifies a transaction in Block 100:
1. The Merkle root of Block 100 changes
2. The block header hash of Block 100 changes
3. Block 101's "Previous Block Hash" field no longer matches
4. Block 101 (and every subsequent block) becomes invalid
5. The attacker must re-mine Block 100 and every subsequent block
6. This requires more computational power than the rest of the network combined

The deeper a block is in the chain (more confirmations), the more computationally expensive it becomes to alter. This is why Bitcoin transactions are considered increasingly secure with each confirmation (most services require 6 confirmations, or approximately 1 hour).

---

## 2.3 Transactions: The UTXO Model

### 2.3.1 Understanding UTXOs

> **Definition: UTXO (Unspent Transaction Output)**
>
> A UTXO is an output from a previous transaction that has not yet been spent (used as an input in a new transaction). The UTXO model is Bitcoin's method of tracking ownership. There are no "account balances" in Bitcoin — instead, a user's balance is the sum of all UTXOs that their private key can unlock. Every Bitcoin transaction consumes (spends) existing UTXOs and creates new ones.

Bitcoin does not use accounts or balances. Instead, it tracks ownership through the UTXO model, which is fundamentally different from the account-based model used by banks and Ethereum.

**Think of UTXOs like physical cash:**
- You have a $20 bill and a $10 bill (two UTXOs worth $30 total)
- You want to pay $25 for dinner
- You hand over both bills ($30 total input)
- You receive $5 in change
- Your two original bills are "spent" (consumed)
- You now have one new bill: the $5 change

In Bitcoin terms:
- **Inputs:** References to UTXOs being spent (the bills you hand over)
- **Outputs:** New UTXOs being created (the payment and your change)
- **Transaction fee:** The difference between total inputs and total outputs goes to the miner

### 2.3.2 Transaction Structure

A Bitcoin transaction contains:

**Inputs (what you're spending):**

| Field | Description |
|-------|-------------|
| Previous Transaction Hash | Hash of the transaction containing the UTXO being spent |
| Output Index | Which output of that transaction (0, 1, 2...) |
| ScriptSig (Unlocking Script) | Proves the spender has the right to spend this UTXO (typically a signature + public key) |
| Sequence Number | Used for transaction replacement (Replace-By-Fee) |

**Outputs (what you're creating):**

| Field | Description |
|-------|-------------|
| Value | Amount in satoshis (1 bitcoin (BTC) = 100,000,000 satoshis) |
| ScriptPubKey (Locking Script) | Conditions that must be met to spend this output (typically requires a signature from a specific public key) |

> **Definition: Satoshi**
>
> A satoshi (sat) is the smallest unit of Bitcoin, equal to 0.00000001 BTC (one hundred-millionth of a bitcoin). Named after Bitcoin's creator, Satoshi Nakamoto. As Bitcoin's price has increased, satoshis have become a practical unit for everyday transactions. 1 BTC = 100,000,000 satoshis.

### 2.3.3 Transaction Example

Consider Alice sending 0.5 BTC to Bob when she has a single UTXO worth 1.0 BTC:

```
INPUT:                               OUTPUTS:
+----------------------------+       +----------------------------+
| Prev Tx: abc123...         |       | Output 0:                  |
| Output Index: 0            |  -->  |   Value: 0.5 BTC           |
| ScriptSig: [Alice's sig]  |       |   ScriptPubKey: [Bob's     |
| (proves Alice owns this   |       |   public key hash]         |
|  UTXO)                    |       +----------------------------+
+----------------------------+       | Output 1:                  |
                                     |   Value: 0.4999 BTC        |
Total Input:  1.0 BTC                |   ScriptPubKey: [Alice's   |
Total Output: 0.9999 BTC             |   public key hash]         |
Fee:          0.0001 BTC             +----------------------------+
```

**What happens:**
1. Alice's 1.0 BTC UTXO is consumed (fully spent — UTXOs cannot be partially spent)
2. Two new UTXOs are created: 0.5 BTC for Bob and 0.4999 BTC as change for Alice
3. The 0.0001 BTC difference is the transaction fee collected by the miner
4. Alice's original UTXO is removed from the UTXO set; two new UTXOs are added

### 2.3.4 The UTXO Set

The UTXO set is the complete collection of all unspent transaction outputs at any given time. It represents the current state of Bitcoin ownership.

**UTXO set properties:**
- As of 2025, the UTXO set contains approximately 80-90 million UTXOs
- Total size: ~5-7 GB (fits in random-access memory (RAM) on modern computers)
- Every full node maintains the UTXO set for fast transaction validation
- To validate a new transaction, a node checks that the referenced UTXOs exist in the set and that the cryptographic conditions (signatures) are satisfied

**UTXO model vs. Account model:**

| Property | UTXO (Bitcoin) | Account (Ethereum) |
|----------|---------------|-------------------|
| State tracking | Set of unspent outputs | Account balances |
| Privacy | New addresses for each transaction are natural | Reusing addresses is the default |
| Parallelism | Transactions spending different UTXOs can be validated in parallel | Transactions from the same account must be ordered (nonce) |
| Complexity | Simple for payments, complex for smart contracts | Natural for smart contracts |
| Storage | UTXO set grows with transaction outputs | State grows with accounts and contract storage |
| Auditability | Can verify total supply by summing all UTXOs | Requires trusted state computation |

> **Notebook Reference:** See `notebooks/02-bitcoin-blockchain-analysis.ipynb` (upcoming) for hands-on UTXO set analysis and transaction parsing.

---

## 2.4 Mining and Proof-of-Work Consensus

### 2.4.1 The Mining Process

> **Definition: Mining**
>
> Bitcoin mining is the process of using computational power to find a valid proof-of-work for a new block. Miners collect pending transactions from the mempool, construct a candidate block, and repeatedly hash the block header with different nonce values until they find a hash that meets the current difficulty target. The successful miner earns the block reward (newly created bitcoin) plus all transaction fees in the block.

Mining serves three critical functions in Bitcoin:
1. **Issuance:** New bitcoins are created through mining rewards (the only way new bitcoin enters circulation)
2. **Security:** The computational cost of mining makes it economically infeasible to attack the network
3. **Consensus:** Mining determines which transactions are confirmed and in what order

**Step-by-step mining process:**

1. **Collect transactions:** The miner selects transactions from the mempool, typically prioritizing those with the highest fees per byte
2. **Construct coinbase transaction:** Create the special first transaction that awards the block reward to the miner's address
3. **Build Merkle tree:** Hash all selected transactions into a Merkle tree and compute the root
4. **Assemble block header:** Combine the previous block hash, Merkle root, timestamp, difficulty target, and an initial nonce
5. **Hash the header:** Compute SHA-256d (double SHA-256) of the 80-byte block header
6. **Check against target:** If the resulting hash, interpreted as a 256-bit number, is less than the target, the block is valid
7. **Iterate:** If not valid, increment the nonce and try again (and modify the coinbase or timestamp if the nonce space is exhausted)
8. **Broadcast:** When a valid hash is found, broadcast the block to the network

### 2.4.2 Difficulty and the Target

> **Definition: Difficulty Target**
>
> The difficulty target is a 256-bit number that a valid block hash must be less than. A lower target means fewer valid hashes exist, making it harder to find one. The target is often described in terms of "leading zeros" — a lower target requires more leading zeros in the hash. Bitcoin adjusts the target every 2,016 blocks to maintain an average block time of approximately 10 minutes.

The difficulty target controls how hard it is to mine a block. The relationship between the target and block hashes:

- SHA-256 produces a 256-bit hash, which can be any value from 0 to 2^256 - 1
- The target defines a threshold: only hashes below this value are valid
- If the target is 2^248, approximately 1 in 2^8 (256) random hashes will be valid
- If the target is 2^240, approximately 1 in 2^16 (65,536) random hashes will be valid

**Difficulty adjustment algorithm:**
Every 2,016 blocks (~2 weeks at 10-minute block times), the network recalculates the difficulty:

```
New Target = Old Target * (Actual Time for Last 2,016 Blocks / Expected Time)

Expected Time = 2,016 blocks * 10 minutes = 20,160 minutes

If blocks were found too fast:  New Target decreases (harder)
If blocks were found too slow:  New Target increases (easier)

Safety limits: Adjustment cannot change by more than a factor of 4 in either direction
```

This self-adjusting mechanism ensures that:
- Block times average ~10 minutes regardless of how much hash power joins or leaves the network
- Bitcoin's issuance schedule remains predictable
- The network adapts to advances in mining hardware

### 2.4.3 Mining Hardware Evolution

Bitcoin mining hardware has evolved through several generations:

**CPU Mining (2009-2010):**
- Satoshi and early adopters mined with standard computer processors
- Hash rate: ~2-20 MH/s (megahashes per second)
- Anyone with a computer could mine profitably

**GPU Mining (2010-2013):**
- Graphics Processing Units (GPUs) are far more efficient at the parallel computations needed for hashing
- Hash rate: ~200-800 MH/s (10-100x improvement over central processing units (CPUs))
- GPU mining rigs became common

**FPGA Mining (2011-2013):**

> **Definition: Field-Programmable Gate Array (FPGA)**
>
> An FPGA is a semiconductor device that can be configured after manufacturing to perform specific computational tasks. For Bitcoin mining, FPGAs were programmed specifically for SHA-256 hashing, offering better energy efficiency than GPUs but less than application-specific integrated circuits (ASICs).

- Hash rate: ~1-5 GH/s (gigahashes per second)
- Better energy efficiency than GPUs
- Bridge between GPU and ASIC era

**ASIC Mining (2013-Present):**

> **Definition: ASIC (Application-Specific Integrated Circuit)**
>
> An ASIC is a microchip designed and manufactured for a single specific purpose. Bitcoin ASICs are designed exclusively to compute SHA-256 hashes as fast and efficiently as possible. They cannot be repurposed for other tasks. Modern Bitcoin ASICs are orders of magnitude more efficient than any general-purpose hardware.

- Hash rate: Modern ASICs achieve 100-300+ TH/s (terahashes per second)
- Energy efficiency: ~20-30 J/TH (joules per terahash)
- Completely dominates Bitcoin mining — CPU/GPU mining is no longer profitable
- Has led to industrial-scale mining operations

**Network hash rate milestones:**

| Year | Approximate Network Hash Rate |
|------|-------------------------------|
| 2009 | < 1 MH/s |
| 2011 | ~10 GH/s |
| 2013 | ~10 TH/s |
| 2015 | ~400 PH/s (petahashes) |
| 2018 | ~40 EH/s (exahashes) |
| 2021 | ~180 EH/s |
| 2025 | ~700+ EH/s |

### 2.4.4 Mining Pools

> **Definition: Mining Pool**
>
> A mining pool is a group of miners who combine their computational resources and share the resulting block rewards proportionally to the hash power each contributed. Mining pools reduce the variance in mining income — instead of a tiny chance of earning a full block reward, participants receive frequent smaller payments.

Solo mining has become impractical for all but the largest operations. Even with a modern ASIC producing 300 TH/s, a solo miner would expect to find a block only once every several years at current network difficulty. Mining pools solve this by aggregating hash power:

**Pool reward distribution methods:**

| Method | Description | Risk Profile |
|--------|-------------|-------------|
| Pay Per Share (PPS) | Fixed payment per valid share submitted, regardless of blocks found | Pool bears variance risk; miner gets steady income |
| Pay Per Last N Shares (PPLNS) | Rewards distributed based on shares contributed in a window around the block found | Shared risk; rewards vary but are higher on average than PPS |
| Full Pay Per Share (FPPS) | Like PPS but also includes estimated transaction fees | Pool bears risk; miner gets the best steady income |

**Mining pool centralization concerns:**
While individual miners retain the ability to switch pools, the concentration of hash power in large pools has raised centralization concerns. If a single pool controlled more than 50% of the hash rate, it could theoretically execute a 51% attack. In practice, miners have historically migrated away from pools approaching this threshold.

> **Notebook Reference:** See `notebooks/07-mining-economics.ipynb` (upcoming) for mining profitability calculations, difficulty simulations, and pool reward modeling.

### 2.4.5 The Coinbase Transaction

Every block contains a special first transaction called the coinbase transaction. Unlike regular transactions, the coinbase transaction:
- Has no inputs (no UTXOs are spent)
- Creates new bitcoin "out of thin air" (the block reward)
- Includes a coinbase field that miners can use for arbitrary data (Satoshi famously embedded the Times headline in the Genesis Block)
- Cannot be spent until 100 blocks have been mined on top of it (the coinbase maturity rule)

The coinbase transaction output includes:
- The block subsidy (currently 3.125 BTC as of 2024)
- All transaction fees from the block's transactions

---

## 2.5 Bitcoin's Economic Model

### 2.5.1 Fixed Supply and Halving Schedule

Bitcoin's monetary policy is entirely predetermined and encoded in the software:

**Maximum supply:** 21,000,000 BTC (exactly 20,999,999.9769 BTC due to rounding)

**Block subsidy halving:** The block reward (subsidy) is cut in half every 210,000 blocks (~4 years):

| Halving | Date | Block Height | Block Subsidy | Cumulative Supply |
|---------|------|-------------|---------------|-------------------|
| 0 (launch) | Jan 2009 | 0 | 50 BTC | 0 |
| 1st | Nov 2012 | 210,000 | 25 BTC | 10,500,000 |
| 2nd | Jul 2016 | 420,000 | 12.5 BTC | 15,750,000 |
| 3rd | May 2020 | 630,000 | 6.25 BTC | 18,375,000 |
| 4th | Apr 2024 | 840,000 | 3.125 BTC | 19,687,500 |
| 5th | ~2028 | 1,050,000 | 1.5625 BTC | 20,343,750 |
| ... | ... | ... | ... | ... |
| 32nd | ~2140 | 6,720,000 | ~1 satoshi | ~21,000,000 |

> **Definition: Halving**
>
> A halving (or "halvening") is a pre-programmed event in Bitcoin where the block subsidy is reduced by 50%. Halvings occur every 210,000 blocks (approximately every four years) and are encoded in Bitcoin's source code. They enforce a disinflationary monetary policy — the rate of new bitcoin creation decreases over time until approximately the year 2140, when the final fractions of a bitcoin will be mined.

**Key economic implications:**
- Over 93% of all bitcoin that will ever exist has already been mined (as of 2025)
- The annual inflation rate decreases with each halving (~0.8% in 2025, approaching 0% by 2140)
- This contrasts fundamentally with fiat currencies, where central banks can increase the money supply without limit
- The scarcity is enforced by code and consensus, not by institutional promise

### 2.5.2 Stock-to-Flow

> **Definition: Stock-to-Flow Ratio (S2F)**
>
> The Stock-to-Flow ratio measures the scarcity of an asset by dividing the existing supply (stock) by the annual production rate (flow). A higher S2F indicates greater scarcity. Gold has an S2F of approximately 60 (it would take 60 years of current production to double the existing supply). Bitcoin's S2F increases with each halving as the flow decreases while the stock grows.

The Stock-to-Flow model, popularized by the pseudonymous analyst PlanB, attempts to value Bitcoin based on its scarcity:

```
Stock-to-Flow = Existing Supply / Annual New Supply

After 4th halving (2024):
  Stock: ~19,700,000 BTC
  Flow:  ~164,250 BTC/year (3.125 BTC * 6 blocks/hour * 24 * 365)
  S2F:   ~120
```

For comparison:
| Asset | Stock-to-Flow |
|-------|:---:|
| Gold | ~60 |
| Silver | ~22 |
| Bitcoin (post-4th halving) | ~120 |

The S2F model has generated significant debate. Proponents argue it demonstrates Bitcoin's increasing scarcity; critics point out that many assets are scarce without being valuable, and that the model's future price predictions rely on assumptions that may not hold.

**Source:** PlanB. (2019). Modeling Bitcoin Value with Scarcity. https://medium.com/@100trillionUSD/modeling-bitcoins-value-with-scarcity-91fa0fc03e25

> **Notebook Reference:** See `notebooks/08-valuation-models.ipynb` (upcoming) for S2F model implementation, Metcalfe's Law, and other Bitcoin valuation frameworks.

### 2.5.3 Transaction Fee Market

As block subsidies decrease with each halving, transaction fees become an increasingly important component of miner revenue. Bitcoin's fee market operates through a first-price auction:

**How fees work:**
- Users include a fee with their transaction, typically measured in satoshis per virtual byte (sat/vB)
- Miners prioritize transactions with higher fee rates
- When demand for block space exceeds the ~4 MB limit, fees rise
- When demand is low, fees can be as little as 1 sat/vB

**Fee estimation:**
- Wallets estimate the fee needed for confirmation within a target number of blocks
- Fee estimates are based on current mempool conditions
- Users can choose between fast (next block), medium (within ~30 minutes), and slow (within ~1 hour) confirmation targets

**Historical fee spikes:**
- December 2017: Average fees exceeded $50 during the bull market
- April 2021: Fees spiked due to transactions related to non-fungible tokens (NFTs)
- May 2023: BRC-20 token minting caused fees to temporarily exceed $30
- These spikes highlight the scalability constraints of Bitcoin's base layer

### 2.5.4 Lost Coins and Effective Supply

A significant portion of bitcoin is estimated to be permanently lost — the private keys needed to spend them have been destroyed, forgotten, or are otherwise inaccessible:

- **Satoshi's coins:** ~1.1 million BTC in wallets attributed to Satoshi have never moved
- **Early mining losses:** Many early miners did not secure their private keys, as bitcoin had negligible value
- **Estimated lost coins:** Various analyses suggest 3-4 million BTC may be permanently lost
- **Effective supply:** The circulating supply available for trade is significantly less than the total mined supply

This permanent loss of coins creates additional deflationary pressure on Bitcoin's already fixed supply.

---

## 2.6 Bitcoin's Scripting Language

### 2.6.1 Bitcoin Script

> **Definition: Bitcoin Script**
>
> Bitcoin Script is a simple, stack-based programming language used to define the conditions under which a UTXO can be spent. It is intentionally not Turing-complete — it has no loops and limited operations — to prevent denial-of-service attacks and keep transaction validation predictable. Each transaction output contains a "locking script" (ScriptPubKey), and each input contains an "unlocking script" (ScriptSig).

Bitcoin transactions use a scripting system to define spending conditions. The two main components are:

**ScriptPubKey (Locking Script):** Placed on the output, defines the conditions to spend
**ScriptSig (Unlocking Script):** Placed on the input, provides the data to satisfy the conditions

**Validation process:**
1. The unlocking script is executed first, pushing data onto the stack
2. The locking script is then executed using the resulting stack
3. If execution completes with a TRUE value on top of the stack, the transaction is valid

### 2.6.2 Common Script Types

**Pay-to-Public-Key-Hash (P2PKH):** The original and most common script type

```
Locking Script:   OP_DUP OP_HASH160 <PubKeyHash> OP_EQUALVERIFY OP_CHECKSIG
Unlocking Script: <Signature> <PublicKey>

Execution:
1. Push Signature and PublicKey to stack
2. OP_DUP: Duplicate the PublicKey
3. OP_HASH160: Hash the duplicate
4. Push the expected PubKeyHash
5. OP_EQUALVERIFY: Check hash matches
6. OP_CHECKSIG: Verify signature against public key
```

These are "Legacy" addresses starting with "1" (e.g., 1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa).

**Pay-to-Script-Hash (P2SH):**

> **Definition: P2SH (Pay-to-Script-Hash)**
>
> P2SH is a script type where the locking condition is the hash of a script, rather than the script itself. The actual spending conditions (the "redeem script") are only revealed when the output is spent. This enables complex scripts (like multisig) while keeping the output small and standardized. P2SH addresses start with "3".

P2SH enables complex scripts while keeping outputs simple. The most common use is multisig:

```
Redeem Script (2-of-3 multisig):
  2 <PubKey1> <PubKey2> <PubKey3> 3 OP_CHECKMULTISIG

Locking Script:
  OP_HASH160 <Hash of Redeem Script> OP_EQUAL

Unlocking Script:
  OP_0 <Sig1> <Sig2> <Redeem Script>
```

**P2WPKH and P2WSH (SegWit):**
Segregated Witness (SegWit) script types, introduced in 2017, separate signature data from the transaction, providing:
- Lower fees (signature data is discounted)
- Transaction malleability fix
- Enables Layer 2 protocols (Lightning Network)
- Bech32 addresses starting with "bc1q"

**P2TR (Taproot):**
Taproot, activated in November 2021, combines Schnorr signatures with Merkelized Alternative Script Trees (MAST):
- Improved privacy (complex scripts look identical to simple payments)
- More efficient multisig (single signature regardless of number of signers)
- Bech32m addresses starting with "bc1p"

### 2.6.3 Multi-Signature (Multisig)

> **Definition: Multi-Signature (Multisig)**
>
> A multisig scheme requires M signatures out of N possible signers to authorize a transaction (written as M-of-N). For example, a 2-of-3 multisig requires any 2 of 3 designated private keys to sign. Multisig is used for enhanced security (no single point of failure), shared custody, and escrow arrangements.

Common multisig configurations:
- **2-of-3:** Commonly used for personal security (e.g., hardware wallet + phone + backup)
- **3-of-5:** Used by exchanges and institutions
- **2-of-2:** Used for escrow or joint accounts

---

## 2.7 Network Security and Attack Vectors

### 2.7.1 The 51% Attack

> **Definition: 51% Attack (Majority Attack)**
>
> A 51% attack occurs when a single entity controls more than half of the network's mining hash rate. With majority hash power, the attacker could: (1) reverse their own transactions (double-spend), (2) prevent other transactions from being confirmed, and (3) prevent other miners from finding blocks. The attacker cannot steal coins from other addresses, create coins out of thin air, or change the protocol rules.

**Economics of a 51% attack on Bitcoin:**
- Acquiring 51% of Bitcoin's hash rate (~700 EH/s) would require:
  - Millions of ASIC miners
  - Massive electricity infrastructure
  - Estimated cost: billions of dollars per day
- A successful attacker would devalue their own investment (the attack would crash Bitcoin's price)
- The attack creates a paradox: the more valuable Bitcoin is, the more expensive the attack, but the less rational it becomes (the attacker destroys value)

### 2.7.2 Sybil Attacks

> **Definition: Sybil Attack**
>
> A Sybil attack occurs when an adversary creates many fake identities (nodes) to gain disproportionate influence over a network. In Bitcoin, Sybil attacks are mitigated by proof-of-work — influence is determined by computational power, not the number of nodes. An attacker can create unlimited nodes, but they cannot mine blocks faster without more hash power.

### 2.7.3 Selfish Mining

Selfish mining is a strategy where a miner with significant hash power (not necessarily 51%) withholds discovered blocks, building a private chain. When the private chain is longer than the public chain, the selfish miner reveals it, causing the honest miners' recent blocks to be orphaned. Research by Eyal and Sirer (2014) showed that selfish mining can be profitable with as little as 25-33% of the hash power under certain conditions.

**Source:** Eyal, I. & Sirer, E. G. (2014). Majority is not Enough: Bitcoin Mining is Vulnerable. https://arxiv.org/abs/1311.0243

### 2.7.4 Eclipse Attacks

An eclipse attack isolates a target node from the rest of the network by monopolizing all of its peer connections. The attacker can then feed the victim node false information about the state of the blockchain. Defenses include peer diversity requirements and connection limits per Internet Protocol (IP) range.

---

## 2.8 Scalability Challenges and Solutions

### 2.8.1 The Scalability Problem

Bitcoin's base layer has fundamental throughput limitations:

| Metric | Bitcoin | Visa (for comparison) |
|--------|---------|----------------------|
| Block time | ~10 minutes | N/A |
| Block size | ~4 MB (weight) | N/A |
| Transactions per second (TPS) | ~7 TPS | ~65,000 TPS |
| Confirmation time | ~10-60 minutes | ~seconds |

This "scalability trilemma" means that increasing throughput requires sacrificing either decentralization or security — or moving transactions to secondary layers.

### 2.8.2 Segregated Witness (SegWit)

> **Definition: Segregated Witness (SegWit)**
>
> SegWit is a protocol upgrade activated on Bitcoin in August 2017 (BIP 141) that separates ("segregates") the digital signature data ("witness") from the transaction data. This provides an effective block size increase (from ~1 MB to ~4 MB of "block weight"), fixes transaction malleability, and enables Layer 2 protocols like the Lightning Network.

SegWit was Bitcoin's first major scaling upgrade. By separating witness data:
- Effective block capacity increased by ~70%
- Transaction malleability was fixed (enabling the Lightning Network)
- Fee discounts for witness data incentivized adoption
- Backward compatible (soft fork — old nodes still work)

### 2.8.3 The Lightning Network

> **Definition: Lightning Network**
>
> The Lightning Network is a Layer 2 payment protocol built on top of Bitcoin. It enables instant, high-volume micropayments by creating bidirectional payment channels between users. Transactions occur off-chain (not recorded on the blockchain individually) and only settle to the Bitcoin blockchain when channels are opened or closed. This dramatically increases throughput and reduces fees.

**How Lightning works:**

1. **Open a channel:** Two parties create a multisig transaction on the Bitcoin blockchain, locking funds in a shared address
2. **Transact off-chain:** The parties exchange signed transactions updating their balances, but do not broadcast them to the blockchain
3. **Route payments:** Payments can be routed through a network of channels — Alice can pay Dave through Bob and Charlie's channels, even without a direct channel to Dave
4. **Close the channel:** Either party can close the channel at any time by broadcasting the latest balance state to the blockchain

**Lightning Network properties:**
- Transactions are nearly instant (milliseconds, not minutes)
- Fees are near-zero (fractions of a cent)
- Throughput: theoretically millions of transactions per second
- Privacy: intermediate routing nodes do not learn the payment endpoints
- Tradeoff: requires users to be online (or use watchtower services) and manage channel liquidity

**Lightning adoption:**
- El Salvador adopted Bitcoin as legal tender in 2021, using Lightning for everyday payments
- Lightning capacity has grown to thousands of BTC
- Integration with major wallets and exchanges (Cash App, Strike, etc.)

**Source:** Poon, J. & Dryja, T. (2016). The Bitcoin Lightning Network: Scalable Off-Chain Instant Payments. https://lightning.network/lightning-network-paper.pdf

### 2.8.4 Block Size Debate and Bitcoin Cash

The scalability debate led to one of Bitcoin's most contentious events. One faction advocated for larger blocks on the base layer; another insisted on keeping blocks small and scaling through Layer 2 solutions.

On August 1, 2017, Bitcoin Cash (BCH) forked from Bitcoin, increasing the block size limit to 8 MB (later 32 MB). The fork demonstrated Bitcoin's governance process:
- No central authority could unilaterally change the rules
- The market ultimately decided which chain was more valuable
- Bitcoin (with small blocks + SegWit + Lightning) retained the vast majority of miners, users, and market value

---

## 2.9 Bitcoin Use Cases and Narratives

### 2.9.1 Digital Gold / Store of Value

The dominant narrative for Bitcoin has shifted from "peer-to-peer electronic cash" (the whitepaper's subtitle) to "digital gold" or "store of value." Proponents argue:

- Fixed supply creates scarcity superior to gold (verifiable, known issuance schedule)
- Easier to store, transport, and divide than physical gold
- Resistant to confiscation (can be stored with only a memorized seed phrase)
- 24/7 global market with no trading hours or settlement delays
- Approximately $1.5+ trillion market capitalization (comparable to major companies, still a fraction of gold's ~$16 trillion)

### 2.9.2 Medium of Exchange

While base-layer Bitcoin is slow and expensive for small payments, the Lightning Network has revived the payment use case:
- Micropayments: Tips, pay-per-article, streaming payments
- Cross-border remittances: Lower fees than traditional services
- Point-of-sale: Growing adoption in El Salvador and other markets
- Machine-to-machine payments: Internet of Things (IoT) and automated systems

### 2.9.3 Censorship-Resistant Money

Bitcoin provides financial access to:
- Individuals in countries with capital controls or currency crises (Venezuela, Argentina, Nigeria)
- Organizations cut off from the banking system
- Dissidents and activists in authoritarian regimes
- The unbanked population globally (~1.4 billion adults without bank accounts)

### 2.9.4 Settlement Layer

Bitcoin's base layer may function as a global settlement layer — a "digital reserve currency" for larger-value transactions, with the Lightning Network and other layers handling everyday payments. This mirrors the traditional financial system where large-value settlement systems (Fedwire, Society for Worldwide Interbank Financial Telecommunication (SWIFT)) underlie consumer payment networks (Visa, PayPal).

---

## Key Takeaways

1. **Bitcoin's P2P network has no hierarchy** — every full node independently validates all transactions and blocks against the consensus rules, creating a trustless system.

2. **The blockchain is a chain of blocks linked by cryptographic hashes.** Modifying any historical transaction requires re-mining that block and all subsequent blocks, which is computationally infeasible with the current network hash rate.

3. **The UTXO model tracks ownership without accounts or balances.** Each transaction consumes existing UTXOs and creates new ones. A user's "balance" is the sum of all UTXOs their private key can unlock.

4. **Mining provides security through economic incentives.** Miners invest real-world resources (hardware, electricity) and are rewarded with newly minted bitcoin and transaction fees, aligning their incentives with network security.

5. **Bitcoin's fixed 21 million supply is enforced by code and consensus.** The halving schedule creates a predictable, disinflationary monetary policy with no central authority able to change it.

6. **The fee market will become increasingly important** as block subsidies decrease with each halving. Transaction fees must eventually sustain network security on their own.

7. **Scalability is addressed through layered architecture** — the base layer provides security and finality, while Layer 2 solutions (Lightning Network) provide speed and low-cost transactions.

8. **Bitcoin's security model relies on economic rationality** — attacking the network costs more than it could plausibly yield, and a successful attack would destroy the value of the attacker's investment.

---

## Further Reading

### Primary Sources
- Nakamoto, S. (2008). Bitcoin: A Peer-to-Peer Electronic Cash System. https://bitcoin.org/bitcoin.pdf
- Poon, J. & Dryja, T. (2016). The Bitcoin Lightning Network. https://lightning.network/lightning-network-paper.pdf

### Books
- Antonopoulos, A. (2017). Mastering Bitcoin, 2nd Edition. O'Reilly Media. https://github.com/bitcoinbook/bitcoinbook
- Song, J. (2019). Programming Bitcoin. O'Reilly Media.
- Narayanan, A. et al. (2016). Bitcoin and Cryptocurrency Technologies. Princeton University Press. https://bitcoinbook.cs.princeton.edu/

### Technical Documentation
- Bitcoin Developer Guide. https://developer.bitcoin.org/
- Bitcoin Improvement Proposals (BIPs). https://github.com/bitcoin/bips
- Bitcoin Core Source Code. https://github.com/bitcoin/bitcoin

### Research
- Eyal, I. & Sirer, E. G. (2014). Majority is not Enough: Bitcoin Mining is Vulnerable. https://arxiv.org/abs/1311.0243
- Bonneau, J. et al. (2015). SoK: Research Perspectives and Challenges for Bitcoin and Cryptocurrencies. IEEE Symposium on Security and Privacy.

---

## Computational Exercises

The following notebooks provide hands-on implementations of concepts covered in this section:

- **`notebooks/01-cryptographic-primitives.ipynb`** — Hash functions, digital signatures, Merkle trees, and proof-of-work that underpin Bitcoin's architecture.

- **`notebooks/02-bitcoin-blockchain-analysis.ipynb`** (upcoming) — Connect to Bitcoin nodes, parse blocks and transactions, analyze the UTXO set, monitor the mempool, and calculate network health metrics.

- **`notebooks/07-mining-economics.ipynb`** (upcoming) — Mining profitability calculations, difficulty adjustment simulations, pool reward modeling, and 51% attack cost estimation.

- **`notebooks/08-valuation-models.ipynb`** (upcoming) — Stock-to-Flow implementation, Metcalfe's Law, network value to transactions (NVT) ratio, and Monte Carlo price simulations.
