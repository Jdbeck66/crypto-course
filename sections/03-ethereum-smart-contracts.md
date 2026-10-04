# Section 3: Ethereum & Smart Contracts - Programmable Blockchains

## Table of Contents

- [3.1 Ethereum Architecture](#31-ethereum-architecture)
- [3.2 The Ethereum Virtual Machine (EVM)](#32-the-ethereum-virtual-machine-evm)
- [3.3 Gas and the Fee Market](#33-gas-and-the-fee-market)
- [3.4 Solidity Programming](#34-solidity-programming)
- [3.5 Smart Contract Security](#35-smart-contract-security)
- [3.6 Token Standards Deep Dive](#36-token-standards-deep-dive)
- [3.7 The Merge: Proof-of-Work to Proof-of-Stake](#37-the-merge-proof-of-work-to-proof-of-stake)
- [3.8 Layer 2 Scaling Solutions](#38-layer-2-scaling-solutions)
- [3.9 The Ethereum Roadmap](#39-the-ethereum-roadmap)
- [Key Takeaways](#key-takeaways)
- [Further Reading](#further-reading)
- [Computational Exercises](#computational-exercises)

---

## 3.1 Ethereum Architecture

### 3.1.1 From Bitcoin to Ethereum

Bitcoin demonstrated that a decentralized, trustless ledger is possible, but its scripting language is intentionally limited. Vitalik Buterin, a programmer and Bitcoin Magazine co-founder, recognized in 2013 that a blockchain with a Turing-complete programming language could serve as a general-purpose platform for decentralized applications — not just money.

> **Definition: Turing-Complete**
>
> A system is Turing-complete if it can simulate any Turing machine — meaning it can compute anything that is computable, given sufficient time and resources. In the context of Ethereum, this means the Ethereum Virtual Machine can execute arbitrary logic, enabling developers to write programs (smart contracts) of essentially unlimited complexity. Bitcoin's Script, by contrast, is intentionally Turing-incomplete (no loops) for security.

Buterin published the Ethereum whitepaper in late 2013, describing "a next-generation smart contract and decentralized application platform." Gavin Wood then authored the Ethereum Yellow Paper in 2014, providing a formal specification of the protocol including the Ethereum Virtual Machine (EVM). The network launched on July 30, 2015, with the "Frontier" release.

**Source:** Buterin, V. (2013). Ethereum Whitepaper: A Next-Generation Smart Contract and Decentralized Application Platform. https://ethereum.org/en/whitepaper/

**Source:** Wood, G. (2014). Ethereum: A Secure Decentralised Generalised Transaction Ledger (Yellow Paper). https://ethereum.github.io/yellowpaper/paper.pdf

### 3.1.2 Account-Based Model

Unlike Bitcoin's Unspent Transaction Output (UTXO) model, Ethereum uses an account-based model similar to a traditional bank ledger. There are two types of accounts:

> **Definition: Externally Owned Account (EOA)**
>
> An Externally Owned Account is an Ethereum account controlled by a private key. EOAs have no associated code and can initiate transactions. Every transaction on Ethereum must originate from an EOA. An EOA's address is derived from the last 20 bytes of the Keccak-256 hash of its public key.

> **Definition: Contract Account**
>
> A Contract Account is an Ethereum account that holds executable code (a smart contract). Contract accounts cannot initiate transactions on their own — they can only execute code in response to a transaction or message call received from an EOA or another contract. Contract accounts have persistent storage that can be modified by their code.

**Every Ethereum account, whether EOA or contract, contains four fields:**

| Field | Description |
|-------|-------------|
| **Nonce** | For EOAs: the number of transactions sent from this account. For contract accounts: the number of contracts created by this account. Prevents replay attacks. |
| **Balance** | The amount of Wei owned by the account. |
| **Storage Hash** | A 256-bit hash of the root of the account's storage trie (empty for EOAs). |
| **Code Hash** | The hash of the EVM code of the account. For EOAs, this is the hash of an empty string. Once deployed, a contract's code is immutable (though proxy patterns can work around this). |

**Key differences between EOAs and Contract Accounts:**

| Property | EOA | Contract Account |
|----------|-----|-----------------|
| Controlled by | Private key | Contract code |
| Can initiate transactions | Yes | No (only responds to calls) |
| Has code | No | Yes |
| Has persistent storage | No | Yes |
| Creation cost | Free (just generate a key pair) | Costs gas to deploy |
| Address derivation | From public key | From creator address + nonce (CREATE) or salt + bytecode (CREATE2) |

### 3.1.3 State and the World State Trie

> **Definition: World State**
>
> The world state is the mapping from every Ethereum address (160-bit identifier) to its account state (nonce, balance, storage hash, code hash). The world state represents the complete current state of the Ethereum network at any given block. Unlike Bitcoin, where state is implicit (derived from the UTXO set), Ethereum explicitly maintains and updates a global state.

Ethereum organizes its state using a Modified Merkle Patricia Trie (MPT), a data structure that combines the properties of a Merkle tree (cryptographic verification) with a Patricia trie (efficient key-value lookups).

> **Definition: Modified Merkle Patricia Trie (MPT)**
>
> A Modified Merkle Patricia Trie is a data structure used by Ethereum to store key-value pairs (account addresses to account states) in a way that is both efficiently searchable and cryptographically verifiable. It combines a radix trie (for path compression) with Merkle hashing (each node's hash depends on its children), enabling proofs that a specific account state exists without downloading the entire state.

**Ethereum maintains four tries per block:**

1. **State Trie** — Maps addresses to account states. The state root is stored in the block header.
2. **Storage Trie** — Each contract account has its own storage trie mapping 256-bit keys to 256-bit values. The root of each storage trie is stored in the account's storage hash field.
3. **Transaction Trie** — Contains all transactions in the block, keyed by their index.
4. **Receipt Trie** — Contains transaction receipts (status, gas used, logs/events), keyed by transaction index.

The **state root** — the root hash of the state trie — is included in every block header. This allows any node to verify the entire state of the network by checking a single 32-byte hash. Light clients can use Merkle proofs against the state root to verify specific account balances or contract storage values without downloading the full state.

**Source:** Wood, G. (2014). Ethereum Yellow Paper, Appendix D: Modified Merkle Patricia Trie. https://ethereum.github.io/yellowpaper/paper.pdf

### 3.1.4 Ethereum vs Bitcoin Architecture Comparison

| Feature | Bitcoin | Ethereum |
|---------|---------|----------|
| **State model** | UTXO (stateless) | Account-based (stateful) |
| **Programming** | Script (limited, stack-based, non-Turing-complete) | Solidity/Vyper (Turing-complete, compiled to EVM bytecode) |
| **Block time** | ~10 minutes | ~12 seconds |
| **Block size** | 4 MB (weight units) | Variable (target 15M gas, max 30M gas) |
| **State storage** | Implicit (UTXO set) | Explicit (world state trie) |
| **Consensus (current)** | Proof-of-Work (SHA-256d) | Proof-of-Stake (Casper Friendly Finality Gadget (FFG) + Latest Message Driven Greedy Heaviest Observed Subtree (LMD-GHOST)) |
| **Supply policy** | Fixed at 21 million bitcoin (BTC) | No hard cap; issuance offset by Ethereum Improvement Proposal 1559 (EIP-1559) fee burning |
| **Native currency** | BTC (8 decimal places) | Ether (ETH; 18 decimal places) |
| **Primary purpose** | Value transfer, store of value | Programmable platform for decentralized applications |
| **Smart contracts** | Very limited (time locks, multisig) | Full-featured (decentralized finance (DeFi), non-fungible tokens (NFTs), decentralized autonomous organizations (DAOs), arbitrary logic) |
| **Transaction finality** | Probabilistic (~6 blocks / 60 min) | Probabilistic + economic finality (~2 epochs / 12.8 min) |

### 3.1.5 Ether (ETH) and Denominations

Ether (ETH) is Ethereum's native cryptocurrency. It serves three purposes:
1. **Payment for computation** — Users pay gas fees in ETH to execute transactions and smart contracts.
2. **Economic security** — Validators stake ETH to participate in consensus; misbehavior results in slashing (loss of staked ETH).
3. **Medium of exchange** — ETH is used as collateral, payment, and a unit of account within the Ethereum ecosystem.

Like Bitcoin's satoshi, ETH has smaller denominations for precise measurement:

| Unit | Wei Value | ETH Value | Common Use |
|------|-----------|-----------|------------|
| **Wei** | 1 | 10^-18 ETH | Smallest unit, used internally |
| **Gwei** (Gigawei) | 10^9 | 10^-9 ETH | Gas prices are quoted in Gwei |
| **Finney** | 10^15 | 10^-3 ETH | Rarely used |
| **Ether (ETH)** | 10^18 | 1 ETH | Standard unit for balances and transfers |

The most commonly encountered denomination beyond ETH is Gwei (Gigawei), where 1 Gwei = 1,000,000,000 Wei = 0.000000001 ETH. Gas prices are nearly always quoted in Gwei — a typical transaction might have a gas price of 20 Gwei.

---

## 3.2 The Ethereum Virtual Machine (EVM)

### 3.2.1 Definition and Purpose

> **Definition: Ethereum Virtual Machine (EVM)**
>
> The Ethereum Virtual Machine is the runtime environment for executing smart contract bytecode on the Ethereum network. It is a quasi-Turing-complete, stack-based virtual machine that operates deterministically — every node that executes the same transaction with the same initial state will produce exactly the same result. The EVM is sandboxed, meaning contract code cannot access the host computer's filesystem, network, or other processes.

The EVM is the computational engine at the heart of Ethereum. Its key properties are:

- **Sandboxed** — Smart contract code executes in a completely isolated environment. A contract cannot access the internet, read files, or interact with anything outside the EVM state. The only way a contract interacts with the outside world is through transactions and oracle contracts.
- **Deterministic** — Given the same input state and transaction, every node will produce exactly the same output state. This is essential for consensus: if execution were non-deterministic, nodes would disagree on state transitions and the network would fork.
- **Stack-based** — The EVM uses a last-in, first-out (LIFO) stack for computation, similar to the Java Virtual Machine (JVM). Operands are pushed onto and popped from the stack.
- **Quasi-Turing-complete** — The EVM can execute any computable function, but execution is bounded by gas. This prevents infinite loops from halting the network.

### 3.2.2 EVM Architecture

The EVM has three data regions for computation:

**1. Stack**
- The primary computation area
- 1024 elements maximum depth
- Each element is 256 bits (32 bytes) wide
- Operations push and pop values from the top of the stack
- Most EVM opcodes operate on the top 1-2 stack elements
- If the stack exceeds 1024 elements or underflows (pop from empty), execution reverts

**2. Memory**
- A byte-addressable, linear array used for temporary data during execution
- Initialized to zero for each new message call
- Volatile — cleared after each external function call
- Expanding memory costs gas: cost grows linearly for the first 724 bytes, then quadratically
- Memory cost formula: `G_memory = 3a + a^2/512` where `a` is the number of 32-byte words allocated

**3. Storage**
- A persistent key-value store mapping 256-bit keys to 256-bit values
- Each contract has its own storage, persisted between transactions
- Organized as a Merkle Patricia Trie (the account's storage trie)
- The most expensive data region: writing to storage costs 20,000 gas (new slot) or 5,000 gas (update)
- Reading from storage costs 2,100 gas (cold access) or 100 gas (warm access, after EIP-2929)

```
┌──────────────────────────────────────────────┐
│                  EVM Execution               │
│                                              │
│  ┌──────────┐  ┌──────────┐  ┌───────────┐  │
│  │  Stack   │  │  Memory  │  │  Storage  │  │
│  │ (LIFO)   │  │ (Temp)   │  │ (Persist) │  │
│  │ 256-bit  │  │ Byte-    │  │ Key-Value │  │
│  │ max 1024 │  │ array    │  │ 256->256  │  │
│  │ elements │  │ volatile │  │ per       │  │
│  │          │  │          │  │ contract  │  │
│  └──────────┘  └──────────┘  └───────────┘  │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │         Program Counter (PC)         │    │
│  │     Points to current opcode in      │    │
│  │        contract bytecode             │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │              Gas Counter             │    │
│  │     Decremented with each opcode     │    │
│  │     Out of gas → execution reverts   │    │
│  └──────────────────────────────────────┘    │
└──────────────────────────────────────────────┘
```

### 3.2.3 Opcodes and Bytecode

> **Definition: Opcode (Operation Code)**
>
> An opcode is a single instruction that the EVM can execute. Each opcode is one byte (values 0x00 to 0xFF), giving a maximum of 256 possible opcodes. Smart contract source code (written in Solidity, Vyper, etc.) is compiled into a sequence of these opcodes — the contract's bytecode — which the EVM interprets and executes.

**Categories of EVM opcodes (selected examples):**

| Category | Opcodes | Gas Cost | Description |
|----------|---------|----------|-------------|
| **Arithmetic** | ADD, MUL, SUB, DIV, MOD, EXP | 3-10 (EXP: 10+) | Basic math on 256-bit integers |
| **Comparison** | LT, GT, EQ, ISZERO | 3 | Compare stack values |
| **Bitwise** | AND, OR, XOR, NOT, SHL, SHR | 3 | Bitwise operations |
| **Hashing** | SHA3 (Keccak-256) | 30 + 6 per word | Cryptographic hash |
| **Stack** | PUSH1-PUSH32, POP, DUP1-DUP16, SWAP1-SWAP16 | 2-3 | Stack manipulation |
| **Memory** | MLOAD, MSTORE, MSTORE8 | 3+ | Read/write memory |
| **Storage** | SLOAD, SSTORE | 2100/20000 | Read/write persistent storage |
| **Environment** | ADDRESS, BALANCE, CALLER, CALLVALUE, GASPRICE | 2-2600 | Access execution context |
| **Control Flow** | JUMP, JUMPI, STOP, RETURN, REVERT | 1-8 | Branching and termination |
| **External** | CALL, DELEGATECALL, STATICCALL, CREATE, CREATE2 | 100-32000+ | Interact with other contracts |
| **Logging** | LOG0-LOG4 | 375+ | Emit events |

**Example: Simple addition compiled to bytecode**

Solidity code:
```solidity
function add(uint256 a, uint256 b) public pure returns (uint256) {
    return a + b;
}
```

Corresponding EVM bytecode sequence (simplified):
```
PUSH1 0x00    // Push 0 onto stack (return data offset)
CALLDATALOAD  // Load first argument (a) from calldata
PUSH1 0x20    // Push 32 (offset for second argument)
CALLDATALOAD  // Load second argument (b) from calldata
ADD           // Pop a and b, push a + b
PUSH1 0x00    // Memory offset for return value
MSTORE        // Store result in memory
PUSH1 0x20    // Return data length (32 bytes)
PUSH1 0x00    // Return data offset
RETURN        // Return the result
```

### 3.2.4 Smart Contract Execution Step by Step

> **Definition: Smart Contract**
>
> A smart contract is a program stored on the Ethereum blockchain that automatically executes when predetermined conditions are met. The term was coined by Nick Szabo in 1994. Unlike traditional contracts that require trusted intermediaries for enforcement, smart contracts are enforced by the EVM — once deployed, their code executes exactly as written, without the possibility of censorship, downtime, or third-party interference (assuming no bugs in the code).

When a user calls a smart contract function, the following occurs:

1. **Transaction creation** — The user's wallet constructs a transaction specifying the contract's address as the recipient, the function to call (encoded as the first 4 bytes of the Keccak-256 hash of the function signature, called the "function selector"), the arguments (ABI-encoded), and a gas limit.

2. **Transaction propagation** — The transaction is broadcast to the network and enters the mempool.

3. **Block inclusion** — A validator selects the transaction from the mempool and includes it in a proposed block.

4. **EVM initialization** — The EVM loads the contract's bytecode and initializes a fresh stack, empty memory, and a pointer to the contract's persistent storage.

5. **Execution** — The EVM executes opcodes sequentially from the program counter:
   - Each opcode consumes gas according to its cost
   - The stack, memory, and storage are modified as the code runs
   - If the contract calls another contract, a new execution context is created (sub-call)
   - External calls can pass value (ETH) and data (function calls)

6. **Gas accounting** — Gas is deducted for each opcode. If gas runs out:
   - All state changes in the current execution context are reverted
   - The transaction is still included in the block (the user still pays for the gas consumed)
   - An "out of gas" error is recorded

7. **State commitment** — If execution succeeds:
   - All state changes (storage updates, balance transfers) are applied to the world state
   - Events (logs) are recorded in the transaction receipt
   - Unused gas is refunded to the sender
   - The new state root is computed

8. **Consensus** — Other validators re-execute the transaction independently and verify they arrive at the same state root.

### 3.2.5 Determinism and Why It Matters

Determinism is the single most critical property of the EVM. Every node in the network must be able to independently execute every transaction and arrive at exactly the same result. Without determinism, nodes would disagree on the state of the blockchain, and consensus would be impossible.

**How the EVM ensures determinism:**
- **No floating-point arithmetic** — The EVM uses only 256-bit integer arithmetic. Floating-point operations can produce different results on different hardware due to rounding differences.
- **No access to external data** — Contracts cannot make Hypertext Transfer Protocol (HTTP) requests, read files, or access random number generators. External data must be brought on-chain by oracle contracts.
- **No multithreading** — All execution is single-threaded and sequential within a transaction. Concurrent execution could produce race conditions.
- **Gas limits** — Execution is bounded, preventing infinite loops or non-terminating programs.
- **Fixed instruction set** — All nodes run the same set of opcodes with identical semantics.

The consequence: any data a smart contract needs from the real world (prices, weather, sports scores) must be provided by an **oracle** — a trusted or decentralized service that writes off-chain data into on-chain storage.

> **Definition: Oracle**
>
> An oracle is a service that provides external (off-chain) data to smart contracts on the blockchain. Since smart contracts cannot access external application programming interfaces (APIs) or data sources directly (doing so would break determinism), oracles act as bridges between the blockchain and the real world. Chainlink is the most widely used decentralized oracle network.

**Source:** Antonopoulos, A. & Wood, G. (2018). Mastering Ethereum. Chapter 13: The Ethereum Virtual Machine. O'Reilly Media. https://github.com/ethereumbook/ethereumbook

---

## 3.3 Gas and the Fee Market

### 3.3.1 Gas as Computational Effort

> **Definition: Gas**
>
> Gas is the unit that measures the computational effort required to execute operations on the Ethereum network. Every opcode, storage read/write, and data byte has a fixed gas cost. Gas serves two purposes: it prevents denial-of-service attacks (spam transactions become expensive) and it compensates validators for the resources they expend to process transactions.

Gas decouples computational cost from the market price of ETH. While the gas cost of an ADD opcode is always 3 gas, the monetary cost depends on the gas price the user is willing to pay. This separation allows protocol designers to set gas costs based on computational resource consumption, independent of ETH's volatile market price.

**Gas costs for common operations:**

| Operation | Gas Cost | Notes |
|-----------|----------|-------|
| Transaction base cost | 21,000 | Every transaction pays this minimum |
| Non-zero calldata byte | 16 | Data sent with a transaction |
| Zero calldata byte | 4 | Zero bytes are cheaper |
| SSTORE (new value) | 20,000 | Writing to a new storage slot |
| SSTORE (update) | 5,000 | Updating an existing slot (was 2,900 after EIP-2929 warm) |
| SLOAD (cold) | 2,100 | First read of a storage slot in a transaction |
| SLOAD (warm) | 100 | Subsequent reads of the same slot |
| ADD / SUB | 3 | Basic arithmetic |
| MUL / DIV | 5 | Multiplication, division |
| SHA3 (Keccak-256) | 30 + 6/word | Hashing |
| CALL (with value) | 9,000 + | Calling another contract with ETH transfer |
| CREATE | 32,000 | Deploying a new contract |
| LOG (event) | 375 + 375/topic + 8/byte | Emitting events |
| SELFDESTRUCT | 5,000 | Removing a contract (deprecated post-Dencun) |

### 3.3.2 Gas Limit, Gas Price, and Gas Used

Three key parameters govern transaction costs:

**Gas Limit** — The maximum amount of gas the sender is willing to consume for the transaction. If execution requires more gas than the limit, the transaction reverts (but the gas is still consumed). Setting the gas limit too low causes the transaction to fail; setting it too high is safe because unused gas is refunded.

**Gas Price** — The amount of ETH (in Wei) the sender is willing to pay per unit of gas. Higher gas prices incentivize validators to include the transaction sooner. After EIP-1559, this is split into a base fee and priority tip (see below).

**Gas Used** — The actual amount of gas consumed by executing the transaction. The user pays for gas used, and the remainder (gas limit - gas used) is refunded.

**Transaction cost formula (pre-EIP-1559):**

```
Transaction Fee = Gas Used x Gas Price
```

**Example:**
- A simple ETH transfer uses 21,000 gas
- At a gas price of 20 Gwei:
- Fee = 21,000 x 20 Gwei = 420,000 Gwei = 0.00042 ETH

**Example (complex contract interaction):**
- A Uniswap token swap uses approximately 150,000 gas
- At a gas price of 50 Gwei:
- Fee = 150,000 x 50 Gwei = 7,500,000 Gwei = 0.0075 ETH

### 3.3.3 EIP-1559: The Fee Market Revolution

> **Definition: EIP-1559**
>
> Ethereum Improvement Proposal 1559, activated in the London hard fork (August 2021), replaced Ethereum's first-price auction fee mechanism with a hybrid system featuring an algorithmically determined base fee that is burned (permanently destroyed) and an optional priority tip paid to validators. This made gas fees more predictable and introduced ETH fee burning, giving Ethereum a deflationary mechanism.

**Before EIP-1559 (first-price auction):**
- Users bid a gas price and hoped it was high enough
- Gas prices were extremely volatile and unpredictable
- Users frequently overpaid or waited long periods
- All fees went to miners

**After EIP-1559:**

The transaction fee is split into two components:

```
Transaction Fee = (Base Fee + Priority Tip) x Gas Used
```

Where:
- **Base Fee** — An algorithmically determined minimum price per gas, set by the protocol. This fee is **burned** (permanently removed from circulation). The base fee adjusts up or down based on network congestion.
- **Priority Tip (Priority Fee)** — An optional additional fee paid directly to the validator as an incentive to include the transaction promptly.
- **Max Fee (maxFeePerGas)** — The absolute maximum the sender is willing to pay per gas. The difference between the max fee and (base fee + tip) is refunded.

**Base fee adjustment algorithm:**

The base fee targets 50% block capacity (15 million gas). If a block is:
- **More than 50% full** — The base fee increases by up to 12.5% for the next block
- **Less than 50% full** — The base fee decreases by up to 12.5% for the next block
- **Exactly 50% full** — The base fee stays the same

Mathematically:

```
base_fee[n+1] = base_fee[n] x (1 + 0.125 x (gas_used[n] - gas_target) / gas_target)
```

Where `gas_target = 15,000,000` (half the 30,000,000 gas maximum).

**Example:**
- Block N uses 20,000,000 gas (target is 15,000,000)
- `base_fee[N+1] = base_fee[N] x (1 + 0.125 x (20M - 15M) / 15M)`
- `base_fee[N+1] = base_fee[N] x 1.04167` (increases ~4.2%)

If blocks are consistently full, the base fee doubles approximately every 6 blocks (since 1.125^6 ≈ 2.03). This exponential increase rapidly prices out low-value transactions during congestion, creating a natural market-clearing mechanism.

### 3.3.4 Impact on Ethereum's Monetary Policy

EIP-1559's fee burning mechanism fundamentally changed ETH's monetary policy:

**Pre-EIP-1559:**
- New ETH issued per block as validator rewards (issuance only, no burning)
- ETH supply was strictly inflationary

**Post-EIP-1559:**
- New ETH is still issued to validators (~1,700 ETH/day post-Merge)
- The base fee of every transaction is burned (removed from supply)
- During periods of high network activity, more ETH is burned than issued — making ETH **deflationary**
- This is sometimes called "ultrasound money" by the Ethereum community

**Net issuance formula:**

```
Net ETH Change = Validator Rewards (issuance) - Base Fees Burned
```

Since the Merge (September 2022), ETH has been net deflationary during periods of moderate-to-high network usage. During low-activity periods, issuance exceeds burning and the supply grows modestly. As of early 2026, the total supply of ETH is slightly below its pre-Merge peak of ~120.5 million ETH.

**Source:** Buterin, V. et al. (2019). EIP-1559: Fee market change for ETH 1.0 chain. https://eips.ethereum.org/EIPS/eip-1559

### 3.3.5 Practical Gas Cost Examples

| Transaction Type | Typical Gas Used | Cost at 20 Gwei | Cost at 100 Gwei |
|-----------------|-----------------|-----------------|------------------|
| Simple ETH transfer | 21,000 | 0.00042 ETH | 0.0021 ETH |
| Ethereum Request for Comments 20 (ERC-20) token transfer | 65,000 | 0.0013 ETH | 0.0065 ETH |
| ERC-20 approval | 46,000 | 0.00092 ETH | 0.0046 ETH |
| Uniswap swap | 150,000 | 0.003 ETH | 0.015 ETH |
| NFT mint (ERC-721) | 150,000-250,000 | 0.003-0.005 ETH | 0.015-0.025 ETH |
| Contract deployment (simple) | 500,000 | 0.01 ETH | 0.05 ETH |
| Contract deployment (complex) | 2,000,000-5,000,000 | 0.04-0.1 ETH | 0.2-0.5 ETH |
| Aave lending deposit | 250,000 | 0.005 ETH | 0.025 ETH |

These costs have driven the adoption of Layer 2 solutions (Section 3.8), where the same operations cost a fraction of the mainnet price.

---

## 3.4 Solidity Programming

### 3.4.1 Language Overview

> **Definition: Solidity**
>
> Solidity is a statically typed, contract-oriented, high-level programming language designed for implementing smart contracts on the Ethereum Virtual Machine. Created by Gavin Wood, Christian Reitwiessner, and others at the Ethereum Foundation, Solidity draws syntactic influence from C++, JavaScript, and Python. It compiles to EVM bytecode that executes on every node in the network.

**Key language characteristics:**
- **Statically typed** — All variable types must be declared at compile time
- **Contract-oriented** — The primary code unit is a `contract`, analogous to a class in object-oriented programming
- **Compiled** — Solidity source code is compiled to EVM bytecode using the `solc` compiler
- **Supports inheritance** — Contracts can inherit from one or more parent contracts (multiple inheritance)
- **Event-driven** — Contracts emit events that external applications can listen for
- **Version-specific** — Solidity source files declare a compiler version pragma (e.g., `pragma solidity ^0.8.20;`)

### 3.4.2 Contract Structure

A Solidity contract consists of several components:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

// Import statements
import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

// Contract definition
contract MyContract {
    // State variables (stored in persistent storage)
    address public owner;
    uint256 public totalDeposits;
    mapping(address => uint256) public balances;

    // Events (logged but not stored in contract storage)
    event Deposit(address indexed user, uint256 amount);
    event Withdrawal(address indexed user, uint256 amount);

    // Errors (custom error types, gas-efficient since Solidity 0.8.4)
    error InsufficientBalance(uint256 requested, uint256 available);
    error Unauthorized();

    // Modifiers (reusable access control logic)
    modifier onlyOwner() {
        if (msg.sender != owner) revert Unauthorized();
        _;  // Placeholder for the function body
    }

    // Constructor (executed once at deployment)
    constructor() {
        owner = msg.sender;
    }

    // Functions
    function deposit() public payable {
        balances[msg.sender] += msg.value;
        totalDeposits += msg.value;
        emit Deposit(msg.sender, msg.value);
    }

    function withdraw(uint256 amount) public {
        if (balances[msg.sender] < amount) {
            revert InsufficientBalance(amount, balances[msg.sender]);
        }
        balances[msg.sender] -= amount;
        payable(msg.sender).transfer(amount);
        emit Withdrawal(msg.sender, amount);
    }

    // View function (reads state but does not modify it, costs no gas when called externally)
    function getBalance(address user) public view returns (uint256) {
        return balances[user];
    }

    // Pure function (neither reads nor modifies state)
    function calculateFee(uint256 amount) public pure returns (uint256) {
        return amount * 3 / 1000;  // 0.3% fee
    }
}
```

### 3.4.3 Data Types

**Value Types** (stored directly, passed by value):

| Type | Description | Example |
|------|-------------|---------|
| `bool` | Boolean | `true`, `false` |
| `uint256` | Unsigned 256-bit integer (also `uint8` through `uint256` in steps of 8) | `42`, `2**255` |
| `int256` | Signed 256-bit integer | `-1`, `100` |
| `address` | 20-byte Ethereum address | `0x742d35Cc...` |
| `address payable` | Address that can receive ETH | `payable(0x742d...)` |
| `bytes1` to `bytes32` | Fixed-size byte arrays | `0xabcdef` |
| `enum` | User-defined enumeration | `enum Status { Active, Paused }` |

**Reference Types** (stored by reference, must specify data location):

| Type | Description | Data Locations |
|------|-------------|---------------|
| `string` | Dynamic-length UTF-8 string | `memory`, `storage`, `calldata` |
| `bytes` | Dynamic-length byte array | `memory`, `storage`, `calldata` |
| `arrays` | Fixed or dynamic-length arrays (`uint256[]`, `uint256[10]`) | `memory`, `storage`, `calldata` |
| `struct` | User-defined composite types | `memory`, `storage`, `calldata` |

**Mapping Type:**

```solidity
mapping(KeyType => ValueType) public myMapping;
```

- Maps keys to values (like a hash table)
- Keys can be any value type; values can be any type including other mappings
- Cannot be iterated (no way to enumerate keys)
- All possible keys exist and map to a default zero value
- Stored only in `storage` (not in `memory`)

**Data locations:**
- **`storage`** — Persistent, on-chain state (expensive to read/write)
- **`memory`** — Temporary, exists only during function execution (cheaper)
- **`calldata`** — Read-only area where function arguments for `external` functions are stored (cheapest)

### 3.4.4 Function Visibility

| Visibility | Accessible From | Use Case |
|-----------|----------------|----------|
| **`public`** | Externally and internally (auto-generates a getter for state variables) | Most common for user-facing functions |
| **`external`** | Only from outside the contract (or via `this.function()`) | Gas-efficient for functions that receive large arrays via calldata |
| **`internal`** | Only from within the contract and derived (inherited) contracts | Helper functions shared with child contracts |
| **`private`** | Only from within the defining contract (not derived contracts) | Implementation details that should not be inherited |

**Important note:** `private` in Solidity does **not** mean the data is secret. All data on the blockchain is publicly readable. `private` only restricts which contracts can call the function or access the variable through the contract's interface. Anyone can read private variables by directly querying contract storage slots.

### 3.4.5 Key Concepts

**Constructors:**
```solidity
constructor(uint256 _initialSupply) {
    owner = msg.sender;
    totalSupply = _initialSupply;
}
```
- Executed exactly once when the contract is deployed
- Cannot be called after deployment
- Often used to set the owner, initialize state, or mint initial tokens

**Inheritance:**
```solidity
contract Ownable {
    address public owner;
    modifier onlyOwner() { require(msg.sender == owner, "Not owner"); _; }
    constructor() { owner = msg.sender; }
}

contract MyToken is Ownable {
    // Inherits owner, onlyOwner modifier, and constructor logic
    function mint() public onlyOwner { /* ... */ }
}
```
- Solidity supports multiple inheritance with C3 linearization
- The `is` keyword declares inheritance
- The `virtual` and `override` keywords control function overriding
- Parent constructors are called in linearized order

**Interfaces:**
```solidity
interface IERC20 {
    function transfer(address to, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
    // ... (no implementation, just function signatures)
}
```
- Define a contract's external API without implementation
- All functions must be `external`
- Cannot have state variables, constructors, or implemented functions
- Essential for standardization (ERC-20, ERC-721, etc.)

### 3.4.6 Annotated ERC-20 Token Example

The following is a complete, annotated implementation of the ERC-20 (Ethereum Request for Comments 20) token standard:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

/**
 * @title SimpleToken
 * @notice A minimal but complete ERC-20 token implementation.
 * @dev Follows the ERC-20 standard as defined in EIP-20.
 *      Production contracts should use OpenZeppelin's audited implementation.
 */
contract SimpleToken {
    // ============================================================
    // STATE VARIABLES (stored in contract storage on-chain)
    // ============================================================

    string public name;       // Human-readable token name (e.g., "Uniswap")
    string public symbol;     // Token ticker (e.g., "UNI")
    uint8 public decimals;    // Number of decimal places (18 is standard, matching ETH)

    uint256 public totalSupply;  // Total number of tokens in existence (in smallest units)

    // Mapping from account address to token balance
    mapping(address => uint256) public balanceOf;

    // Mapping from owner to spender to approved amount
    // allowance[owner][spender] = amount the spender can transfer on behalf of the owner
    mapping(address => mapping(address => uint256)) public allowance;

    // ============================================================
    // EVENTS (logged to the blockchain, not stored in contract storage)
    // ============================================================

    // Emitted when tokens are transferred (including minting when `from` is address(0))
    event Transfer(address indexed from, address indexed to, uint256 value);

    // Emitted when an owner approves a spender to transfer tokens on their behalf
    event Approval(address indexed owner, address indexed spender, uint256 value);

    // ============================================================
    // CONSTRUCTOR (executed once at deployment)
    // ============================================================

    /**
     * @param _name Token name
     * @param _symbol Token symbol
     * @param _initialSupply Total supply (in whole tokens; will be multiplied by 10^decimals)
     */
    constructor(string memory _name, string memory _symbol, uint256 _initialSupply) {
        name = _name;
        symbol = _symbol;
        decimals = 18;

        // Mint the entire initial supply to the deployer
        // If _initialSupply is 1000, totalSupply becomes 1000 * 10^18 = 1e21 smallest units
        totalSupply = _initialSupply * 10 ** uint256(decimals);
        balanceOf[msg.sender] = totalSupply;

        emit Transfer(address(0), msg.sender, totalSupply);
    }

    // ============================================================
    // CORE ERC-20 FUNCTIONS
    // ============================================================

    /**
     * @notice Transfer `_value` tokens from the caller to `_to`.
     * @param _to Recipient address
     * @param _value Amount in smallest units (wei-equivalent for this token)
     * @return success Always true if the transaction does not revert
     */
    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(_to != address(0), "Transfer to zero address");
        require(balanceOf[msg.sender] >= _value, "Insufficient balance");

        balanceOf[msg.sender] -= _value;  // Safe from underflow in Solidity >=0.8.0
        balanceOf[_to] += _value;

        emit Transfer(msg.sender, _to, _value);
        return true;
    }

    /**
     * @notice Approve `_spender` to transfer up to `_value` tokens on the caller's behalf.
     * @dev This enables the "approve + transferFrom" pattern used by DEXes and DeFi protocols.
     *      WARNING: Changing an allowance from non-zero to non-zero is vulnerable to a
     *      front-running attack. Mitigate by first setting to 0, then to the desired value.
     */
    function approve(address _spender, uint256 _value) public returns (bool success) {
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    /**
     * @notice Transfer `_value` tokens from `_from` to `_to`, using the caller's allowance.
     * @dev The caller must have been approved by `_from` for at least `_value` tokens.
     *      This is how DEXes, lending protocols, and other contracts move tokens
     *      on behalf of users.
     */
    function transferFrom(
        address _from,
        address _to,
        uint256 _value
    ) public returns (bool success) {
        require(_to != address(0), "Transfer to zero address");
        require(balanceOf[_from] >= _value, "Insufficient balance");
        require(allowance[_from][msg.sender] >= _value, "Allowance exceeded");

        balanceOf[_from] -= _value;
        balanceOf[_to] += _value;
        allowance[_from][msg.sender] -= _value;

        emit Transfer(_from, _to, _value);
        return true;
    }
}
```

**Source:** Vogelsteller, F. & Buterin, V. (2015). EIP-20: Token Standard. https://eips.ethereum.org/EIPS/eip-20

---

## 3.5 Smart Contract Security

### 3.5.1 Why Smart Contract Security Is Critical

Smart contracts are immutable once deployed — if the code has a bug, it cannot be patched (without proxy patterns or migration strategies). They often hold significant value: as of 2026, DeFi protocols collectively manage over $100 billion in smart contract deposits. A single vulnerability can result in the instant, irreversible loss of all funds.

The history of Ethereum is punctuated by major exploits that have shaped security best practices.

### 3.5.2 Reentrancy

> **Definition: Reentrancy Attack**
>
> A reentrancy attack occurs when a malicious contract calls back into the vulnerable contract before the first execution is complete, exploiting the fact that state variables have not yet been updated. The attacker can repeatedly drain funds by re-entering the withdrawal function before the balance is decremented.

**The DAO Attack (June 2016):**

The Decentralized Autonomous Organization (DAO) was an investment fund governed by smart contract code, holding approximately $150 million in ETH raised through a token sale. An attacker exploited a reentrancy vulnerability in the DAO's `withdraw` function to drain 3.6 million ETH (~$60 million at the time).

**Vulnerable pattern:**
```solidity
// VULNERABLE - Do not use
function withdraw() public {
    uint256 amount = balances[msg.sender];
    // Step 1: Send ETH to the caller (EXTERNAL CALL)
    (bool success, ) = msg.sender.call{value: amount}("");
    require(success, "Transfer failed");
    // Step 2: Update balance AFTER the external call
    balances[msg.sender] = 0;  // Too late! The attacker already re-entered
}
```

**How the attack works:**
1. Attacker deploys a malicious contract with a `receive()` function that calls `withdraw()` again
2. Attacker calls `withdraw()` on the vulnerable contract
3. The vulnerable contract sends ETH to the attacker's contract
4. The attacker's `receive()` function is triggered by the incoming ETH
5. Inside `receive()`, the attacker calls `withdraw()` again
6. Since `balances[msg.sender]` has not been updated yet (still at the original amount), the check passes
7. The contract sends ETH again, and the cycle repeats until the contract is drained

**The fix — Checks-Effects-Interactions (CEI) pattern:**
```solidity
// SECURE - Checks-Effects-Interactions pattern
function withdraw() public {
    uint256 amount = balances[msg.sender];
    require(amount > 0, "No balance");      // CHECK

    balances[msg.sender] = 0;                // EFFECT (update state BEFORE external call)

    (bool success, ) = msg.sender.call{value: amount}("");  // INTERACTION (external call last)
    require(success, "Transfer failed");
}
```

The DAO attack led to a controversial hard fork of Ethereum to reverse the theft, resulting in Ethereum (ETH) and Ethereum Classic (ETC) as separate chains.

**Source:** Siegel, D. (2016). Understanding The DAO Attack. CoinDesk. https://www.coindesk.com/learn/understanding-the-dao-attack/

### 3.5.3 Integer Overflow and Underflow

> **Definition: Integer Overflow/Underflow**
>
> An integer overflow occurs when an arithmetic operation produces a value larger than the maximum value the type can hold, causing it to wrap around to a small number. An integer underflow occurs when a subtraction produces a value below zero for an unsigned integer, wrapping to a very large number. In Solidity versions prior to 0.8.0, these wrapping behaviors occurred silently without error.

**Example (pre-Solidity 0.8.0):**
```solidity
// In Solidity <0.8.0:
uint8 x = 255;
x += 1;  // x is now 0 (overflow: 255 + 1 wraps to 0)

uint8 y = 0;
y -= 1;  // y is now 255 (underflow: 0 - 1 wraps to 255)
```

This vulnerability was exploited in several token contracts where attackers could generate tokens from nothing by triggering underflows in balance calculations.

**Mitigation:**
- Solidity 0.8.0+ includes **built-in overflow/underflow checks** — arithmetic operations that overflow or underflow automatically revert the transaction.
- For Solidity <0.8.0, the OpenZeppelin `SafeMath` library was the standard defense.
- If unchecked arithmetic is intentionally needed (for gas savings in known-safe operations), Solidity 0.8.0+ provides the `unchecked { }` block.

### 3.5.4 Front-Running

> **Definition: Front-Running**
>
> Front-running is the practice of observing a pending transaction in the mempool and submitting a competing transaction with a higher gas price to ensure it is executed first. On Ethereum, all pending transactions are visible in the mempool before they are included in a block, creating opportunities for Maximal Extractable Value (MEV) extraction.

**Example scenario:**
1. Alice submits a large buy order on a decentralized exchange (DEX), visible in the mempool
2. A front-runner bot sees Alice's transaction and submits a buy order with a higher gas price
3. The bot's buy order executes first, raising the price
4. Alice's buy order executes at the higher price
5. The bot sells immediately after Alice's transaction, pocketing the price difference

This is sometimes called a **sandwich attack** when the attacker places a transaction both before and after the victim's transaction.

**Mitigation strategies:**
- **Private transaction pools** (Flashbots Protect, MEV Blocker) — submit transactions directly to block builders, bypassing the public mempool
- **Commit-reveal schemes** — submit a hash of the transaction first, then reveal the details later
- **Slippage limits** — set maximum acceptable price impact on DEX trades
- **Batch auctions** — execute all orders at a uniform price (used by CoW Protocol)

### 3.5.5 Access Control Issues

Improperly implemented access control allows unauthorized users to call privileged functions:

```solidity
// VULNERABLE: No access control on critical function
function mint(address to, uint256 amount) public {
    totalSupply += amount;
    balanceOf[to] += amount;
}

// SECURE: Restricted to contract owner
function mint(address to, uint256 amount) public onlyOwner {
    totalSupply += amount;
    balanceOf[to] += amount;
}
```

Common access control mistakes include:
- Missing `onlyOwner` or role-based modifiers on sensitive functions
- Using `tx.origin` instead of `msg.sender` for authorization (vulnerable to phishing attacks)
- Leaving `initialize()` functions unprotected in upgradeable proxy contracts
- Default visibility: functions without explicit visibility were `public` in older Solidity versions

### 3.5.6 Oracle Manipulation

> **Definition: Oracle Manipulation**
>
> Oracle manipulation attacks exploit smart contracts that rely on easily manipulable price feeds. If a contract uses a single DEX pool's spot price as an oracle, an attacker can manipulate that price with a large trade, exploit the contract at the manipulated price, then reverse the trade — often within a single transaction using flash loans.

**Example:** A lending protocol uses a Uniswap pool's current price to determine collateral value. An attacker:
1. Takes a flash loan of millions of dollars
2. Dumps tokens into the Uniswap pool, crashing the price
3. Liquidates undercollateralized positions on the lending protocol at the manipulated price
4. Buys back the tokens cheaply
5. Repays the flash loan, keeping the profit — all in one transaction

**Mitigation:**
- Use **time-weighted average prices (TWAPs)** instead of spot prices
- Use **decentralized oracle networks** (Chainlink) that aggregate data from multiple sources
- Implement **price deviation checks** that reject sudden large price changes
- Use **multiple oracle sources** and require consensus

### 3.5.7 Security Best Practices

**The Checks-Effects-Interactions (CEI) Pattern:**
1. **Checks** — Validate all inputs and preconditions (require statements)
2. **Effects** — Update all state variables
3. **Interactions** — Make external calls (transfers, contract calls) last

**Additional best practices:**
- Use **OpenZeppelin contracts** — battle-tested, audited implementations of common patterns (ERC-20, access control, reentrancy guards, etc.)
- Apply the **principle of least privilege** — functions should have the minimum necessary permissions
- Use **reentrancy guards** (`nonReentrant` modifier from OpenZeppelin)
- Prefer **pull over push payments** — let users withdraw funds rather than sending to them
- **Limit the attack surface** — keep contracts small and modular
- **Fail safely** — default to denying access, not granting it

**Formal Verification and Auditing:**

> **Definition: Formal Verification**
>
> Formal verification is the process of mathematically proving that a smart contract's code satisfies a formal specification of its intended behavior. Unlike testing (which checks specific cases), formal verification exhaustively proves properties hold for all possible inputs. Tools like Certora, Halmos, and the Solidity SMTChecker enable formal verification of Solidity contracts.

The smart contract security ecosystem includes:
- **Audit firms** — Trail of Bits, OpenZeppelin, Consensys Diligence, Spearbit, Cantina
- **Bug bounty platforms** — Immunefi (offering bounties up to $10 million for critical vulnerabilities)
- **Static analysis tools** — Slither, Mythril, Securify
- **Fuzzing tools** — Foundry (fuzz testing), Echidna (property-based testing)
- **Formal verification** — Certora Prover, Halmos, K Framework

**Source:** Atzei, N., Bartoletti, M., & Cimoli, T. (2017). A Survey of Attacks on Ethereum Smart Contracts. Principles of Security and Trust (POST). https://eprint.iacr.org/2016/1007

---

## 3.6 Token Standards Deep Dive

### 3.6.1 ERC-20: Fungible Tokens

> **Definition: ERC-20**
>
> ERC-20 (Ethereum Request for Comments 20) is the technical standard for fungible tokens on Ethereum, proposed by Fabian Vogelsteller and Vitalik Buterin in November 2015. Fungible means every token is identical and interchangeable — one USD Coin (USDC) is always equal to any other USDC, just as one dollar bill is equal to any other dollar bill. ERC-20 defines a common interface that all fungible tokens implement, enabling interoperability across wallets, exchanges, and DeFi protocols.

**Required interface:**

| Function | Description |
|----------|-------------|
| `totalSupply()` | Returns the total number of tokens in existence |
| `balanceOf(address)` | Returns the token balance of a specific address |
| `transfer(to, amount)` | Transfers tokens from the caller to another address |
| `approve(spender, amount)` | Authorizes a spender to transfer up to `amount` tokens on the caller's behalf |
| `transferFrom(from, to, amount)` | Transfers tokens from one address to another, using the caller's allowance |
| `allowance(owner, spender)` | Returns the remaining allowance a spender has for an owner |

**Required events:**
- `Transfer(from, to, value)` — Emitted on every transfer (including mints and burns)
- `Approval(owner, spender, value)` — Emitted when an allowance is set

**Use cases:** Stablecoins (USDC, Tether (USDT), DAI), governance tokens (Uniswap (UNI), AAVE, Maker (MKR)), utility tokens, wrapped assets (wrapped ether (WETH), Wrapped Bitcoin (WBTC)).

**Limitations:**
- The approve/transferFrom pattern requires two transactions for a contract to spend tokens on a user's behalf
- Tokens sent directly to a contract that does not expect them can be permanently lost
- No built-in callback mechanism (addressed by ERC-777, though it introduced reentrancy risks)

### 3.6.2 ERC-721: Non-Fungible Tokens (NFTs)

> **Definition: ERC-721 (Non-Fungible Token / NFT)**
>
> ERC-721 is the standard for non-fungible tokens on Ethereum, proposed by William Entriken, Dieter Shirley, Jacob Evans, and Nastassia Sachs in 2018. Unlike ERC-20 tokens, each ERC-721 token has a unique `tokenId` and is not interchangeable with any other token. This standard is used for digital art, collectibles, gaming items, domain names, real-world asset representations, and any asset that requires unique identification.

**Core interface:**

| Function | Description |
|----------|-------------|
| `balanceOf(owner)` | Number of NFTs owned by an address |
| `ownerOf(tokenId)` | Returns the owner of a specific NFT |
| `safeTransferFrom(from, to, tokenId)` | Transfers an NFT, checking that the recipient can handle it |
| `transferFrom(from, to, tokenId)` | Transfers an NFT (no safety check) |
| `approve(to, tokenId)` | Approves another address to transfer a specific NFT |
| `setApprovalForAll(operator, approved)` | Approves an operator for all of the caller's NFTs |
| `getApproved(tokenId)` | Returns the approved address for a specific NFT |
| `isApprovedForAll(owner, operator)` | Checks if an operator is approved for all of an owner's NFTs |

**Metadata extension (ERC-721Metadata):**
```solidity
function name() external view returns (string);
function symbol() external view returns (string);
function tokenURI(uint256 tokenId) external view returns (string);
```

The `tokenURI` function returns a Uniform Resource Identifier (URI) pointing to a JavaScript Object Notation (JSON) file with metadata:
```json
{
    "name": "CryptoPunk #7804",
    "description": "One of 10,000 unique CryptoPunks.",
    "image": "ipfs://QmXkY...",
    "attributes": [
        { "trait_type": "Type", "value": "Alien" },
        { "trait_type": "Accessory", "value": "Pipe" }
    ]
}
```

**Enumeration extension (ERC-721Enumerable):**
- `totalSupply()` — Total number of NFTs
- `tokenByIndex(index)` — Returns the tokenId at a given index across all NFTs
- `tokenOfOwnerByIndex(owner, index)` — Returns the tokenId at a given index for a specific owner

**Source:** Entriken, W. et al. (2018). EIP-721: Non-Fungible Token Standard. https://eips.ethereum.org/EIPS/eip-721

### 3.6.3 ERC-1155: Multi-Token Standard

> **Definition: ERC-1155**
>
> ERC-1155 is a multi-token standard that allows a single contract to manage multiple token types — both fungible and non-fungible — within one contract. Created by Witek Radomski (Enjin) in 2018, it is optimized for batch operations, reducing gas costs by up to 90% compared to deploying separate ERC-20 and ERC-721 contracts. It is widely used in gaming, where a single game may have currencies (fungible), unique items (non-fungible), and semi-fungible items (limited editions).

**Key advantages over ERC-20 + ERC-721:**

| Feature | ERC-20 / ERC-721 | ERC-1155 |
|---------|------------------|----------|
| Tokens per contract | One | Unlimited |
| Batch transfers | No (one tx per token) | Yes (transfer many token types in one tx) |
| Gas for N token types | N deployments | 1 deployment |
| Mixed fungible + non-fungible | Requires separate contracts | Single contract handles both |
| Batch balance queries | N calls | 1 call (`balanceOfBatch`) |

**Core functions:**
- `balanceOf(account, id)` — Balance of a specific token type
- `balanceOfBatch(accounts[], ids[])` — Batch balance query
- `safeTransferFrom(from, to, id, amount, data)` — Transfer a specific token type
- `safeBatchTransferFrom(from, to, ids[], amounts[], data)` — Transfer multiple token types in one transaction

**Use cases:** Blockchain games (swords, potions, gold all in one contract), NFT collections with editions, tokenized real-world asset portfolios.

**Source:** Radomski, W. et al. (2018). EIP-1155: Multi Token Standard. https://eips.ethereum.org/EIPS/eip-1155

### 3.6.4 ERC-4626: Tokenized Vault Standard

> **Definition: ERC-4626**
>
> ERC-4626 is a standard for tokenized vaults — contracts that accept deposits of an underlying ERC-20 token and issue "vault shares" representing the depositor's proportional claim on the underlying assets plus any yield. Proposed in 2022, it standardizes the interface for yield-bearing tokens, enabling DeFi composability: any protocol that supports ERC-4626 can integrate any compliant vault without custom adapters.

**How it works:**
1. User deposits 100 USDC into a vault
2. Vault issues 100 vault shares (initially 1:1)
3. The vault deploys the USDC into a yield strategy (e.g., lending on Aave)
4. Over time, the vault accrues interest — now holds 110 USDC
5. Each share is now worth 1.1 USDC
6. User redeems 100 shares and receives 110 USDC

**Key interface (extends ERC-20):**

| Function | Description |
|----------|-------------|
| `asset()` | Returns the address of the underlying token (e.g., USDC) |
| `deposit(assets, receiver)` | Deposits underlying tokens, mints vault shares |
| `mint(shares, receiver)` | Mints exact number of shares, pulls required underlying |
| `withdraw(assets, receiver, owner)` | Burns shares, returns exact amount of underlying |
| `redeem(shares, receiver, owner)` | Burns exact number of shares, returns underlying |
| `totalAssets()` | Total underlying tokens managed by the vault |
| `convertToShares(assets)` | Preview how many shares for a given deposit |
| `convertToAssets(shares)` | Preview how much underlying for a given redemption |

**Conversion formulas:**

```
shares = deposit_amount x total_shares / total_assets
assets = redeem_shares x total_assets / total_shares
```

**DeFi composability:** Because every ERC-4626 vault exposes the same interface, protocols can generically interact with any vault. A yield aggregator like Yearn can route funds to any ERC-4626 vault, and a lending protocol can accept any ERC-4626 shares as collateral — all without custom integration code.

**Source:** Joey Santoro et al. (2022). EIP-4626: Tokenized Vault Standard. https://eips.ethereum.org/EIPS/eip-4626

### 3.6.5 When to Use Which Standard

| Scenario | Recommended Standard | Rationale |
|----------|---------------------|-----------|
| Currency / stablecoin | ERC-20 | Fungible, simple, universally supported |
| Unique digital art | ERC-721 | Each piece is unique, needs individual metadata |
| Gaming (mixed items) | ERC-1155 | Multiple item types, batch operations, gas efficiency |
| Yield-bearing position | ERC-4626 | Standardized vault interface, DeFi composability |
| Membership / access pass | ERC-721 or ERC-1155 | Depends on whether passes are unique or identical |
| Real-world asset (unique) | ERC-721 | Each asset (property, bond) is unique |
| Real-world assets (fractional) | ERC-20 + ERC-721 | ERC-721 for the asset, ERC-20 for fractional shares |
| Semi-fungible (event tickets) | ERC-1155 | Sections are fungible, seats are non-fungible |

---

## 3.7 The Merge: Proof-of-Work to Proof-of-Stake

### 3.7.1 Why Ethereum Transitioned

Ethereum launched in 2015 with a Proof-of-Work (PoW) consensus mechanism similar to Bitcoin, using the Ethash mining algorithm. The plan to transition to Proof-of-Stake (PoS) was part of Ethereum's roadmap from the very beginning — the original whitepaper mentioned it, and the difficulty bomb (a mechanism that gradually makes PoW mining harder) was built into the protocol to incentivize the transition.

**Motivations for the transition:**

1. **Energy consumption** — Ethereum's PoW consumed approximately 112 TWh/year at its peak (comparable to the Netherlands), primarily due to graphics processing unit (GPU) mining. PoS reduced this by ~99.95%.

2. **Centralization pressure** — PoW mining was increasingly dominated by large operations with access to cheap electricity and specialized hardware. PoS allows anyone with 32 ETH and consumer hardware to validate.

3. **Economic security** — PoS achieves stronger economic security per dollar spent. Attacking a PoS network requires acquiring and staking a large amount of ETH, which would lose value if the attack succeeded (unlike PoW mining hardware, which retains value across chains).

4. **Scalability foundation** — PoS enables future scalability upgrades (sharding, danksharding) that are difficult or impossible with PoW.

### 3.7.2 The Beacon Chain and the Merge Process

The transition occurred in two phases:

**Phase 1: Beacon Chain Launch (December 1, 2020)**

> **Definition: Beacon Chain**
>
> The Beacon Chain is the Proof-of-Stake consensus layer of Ethereum, launched on December 1, 2020, as a separate chain running alongside the existing PoW chain. It managed the validator registry, assigned validators to committees, applied rewards and penalties, and implemented the Casper FFG and LMD-GHOST fork choice rule. At the Merge, the Beacon Chain became the consensus engine for the entire Ethereum network.

- The Beacon Chain ran in parallel with the PoW chain for 21 months
- Validators deposited 32 ETH into a one-way deposit contract on the PoW chain
- The Beacon Chain managed consensus without processing any user transactions
- This parallel operation allowed the PoS mechanism to be tested with real stakes

**Phase 2: The Merge (September 15, 2022)**

The Merge was the event where Ethereum's execution layer (transactions, smart contracts, state) joined with the Beacon Chain's consensus layer (PoS). At a specific Total Terminal Difficulty (TTD) value of 58,750,000,000,000,000,000,000, the PoW chain stopped producing blocks and the Beacon Chain took over block production.

**Key facts about the Merge:**
- No downtime — the transition was seamless, with zero interruption to the network
- No change to user experience — transactions, addresses, smart contracts, and decentralized applications (DApps) continued to function identically
- Mining ceased permanently — Ethereum miners' GPU hardware could no longer produce blocks
- Block time changed from variable (~13 seconds average) to fixed 12-second slots
- Considered one of the most complex live infrastructure upgrades in software history

**Source:** Ethereum Foundation. (2022). The Merge. https://ethereum.org/en/roadmap/merge/

### 3.7.3 Proof-of-Stake Mechanics

> **Definition: Proof-of-Stake (PoS)**
>
> Proof-of-Stake is a consensus mechanism where validators are selected to propose and attest to blocks based on the amount of cryptocurrency they have "staked" (locked as collateral). Instead of competing through computational work (PoW), validators are chosen pseudo-randomly, weighted by their stake. Validators who behave honestly earn rewards; those who misbehave lose part of their stake through "slashing."

**Ethereum's PoS implementation uses two protocols:**

1. **Casper FFG (Friendly Finality Gadget)** — Provides finality by having validators vote on epoch checkpoints. When 2/3 of staked ETH votes for a checkpoint, it becomes "justified." When two consecutive checkpoints are justified, the earlier one becomes "finalized" (irreversible).

2. **LMD-GHOST (Latest Message Driven Greedy Heaviest Observed Sub-Tree)** — The fork choice rule that determines the canonical chain. Validators vote for the block they consider the head of the chain, and the chain with the most validator support (weighted by stake) is chosen.

### 3.7.4 Validators and Staking

**Becoming a validator:**
1. Deposit exactly 32 ETH into the deposit contract (the minimum stake)
2. Run an execution client (Geth, Nethermind, Besu, Erigon) and a consensus client (Prysm, Lighthouse, Teku, Nimbus, Lodestar)
3. Maintain uptime — the validator must be online to perform duties
4. Hardware requirements: consumer-grade (4+ core central processing unit (CPU), 16+ GB random-access memory (RAM), 2+ TB solid-state drive (SSD), stable internet)

**Validator economics:**

| Parameter | Value |
|-----------|-------|
| Minimum stake | 32 ETH |
| Annual yield (approximate) | 3-5% annual percentage rate (APR; varies with total staked and network activity) |
| Source of rewards | Issuance + priority tips + MEV |
| Withdrawal delay | Variable queue, typically minutes to days |

**Rewards are earned for:**
- **Block proposals** — A validator is randomly selected to propose a block (probability proportional to stake)
- **Attestations** — Validators vote on the correct head of the chain and on epoch checkpoints
- **Sync committee participation** — A rotating committee that helps light clients verify the chain

**Penalties are incurred for:**
- **Inactivity** — Failing to attest costs a small penalty (grows larger during prolonged inactivity)
- **Slashing** — Malicious behavior (double voting, surround voting) results in a significant portion of the stake being destroyed and the validator being forcibly exited

### 3.7.5 Attestations, Committees, and Epochs

> **Definition: Epoch**
>
> An epoch is a period of 32 slots (6.4 minutes at 12 seconds per slot). At the beginning of each epoch, the entire validator set is pseudo-randomly shuffled and divided into committees assigned to each slot. Epochs are the unit over which finality checkpoints are evaluated.

> **Definition: Slot**
>
> A slot is a 12-second window during which one validator is selected to propose a block and a committee of validators is assigned to attest to (vote on) the proposed block. Not every slot necessarily has a block — if the proposer is offline, the slot is missed.

**The attestation process:**
1. Each epoch, all active validators are divided into 32 groups (one per slot)
2. Within each slot, a committee of validators is formed
3. One validator from the committee is chosen as the block proposer
4. The proposer creates a block and broadcasts it
5. Committee members verify the block and publish attestations
6. Attestations include votes on:
   - The proposed block (head vote / LMD-GHOST)
   - The epoch checkpoint (source and target votes / Casper FFG)

**Finality timeline:**
- A block is proposed and attested within its slot (12 seconds)
- After one epoch (6.4 minutes), the checkpoint may be "justified" (2/3 vote)
- After two consecutive justified checkpoints (~12.8 minutes), the earlier is "finalized"
- Finalized blocks cannot be reverted without 1/3 of all staked ETH being slashed

### 3.7.6 Impact of the Merge

**Energy reduction:**
- Pre-Merge: ~112 TWh/year (comparable to the Netherlands)
- Post-Merge: ~0.01 TWh/year
- Reduction: **99.95%**
- Ethereum's carbon footprint decreased from millions of tons of CO2 to negligible levels

**Issuance reduction:**
- Pre-Merge (PoW + PoS): ~13,000 ETH/day issuance
- Post-Merge (PoS only): ~1,700 ETH/day issuance
- Combined with EIP-1559 fee burning: ETH has been periodically net deflationary

**Security model change:**
- Cost to attack: acquiring 33% of staked ETH (tens of billions of dollars) vs. acquiring 51% of hash power
- Slashing: attackers lose their staked ETH, making attacks directly punishing
- No arms race: no need for increasingly specialized and expensive hardware

### 3.7.7 Liquid Staking

> **Definition: Liquid Staking**
>
> Liquid staking is a mechanism that allows users to stake their ETH and receive a transferable, DeFi-compatible token (a Liquid Staking Token, or LST) representing their staked position. This solves the liquidity problem: without liquid staking, staked ETH is locked and cannot be used as collateral, traded, or otherwise deployed in DeFi.

**Major liquid staking protocols:**

| Protocol | Token | Mechanism | Market Share |
|----------|-------|-----------|-------------|
| **Lido** | stETH | Pooled staking via curated node operators; stETH rebases daily to reflect staking rewards | ~28% of staked ETH |
| **Rocket Pool** | rETH | Permissionless node operator set; rETH appreciates in value against ETH | ~3% of staked ETH |
| **Coinbase** | cbETH | Centralized exchange staking; cbETH appreciates against ETH | ~3% of staked ETH |
| **Frax** | sfrxETH | Dual-token model (frxETH + sfrxETH) optimizing for DeFi utility | ~1% of staked ETH |

**stETH (Lido) pricing:**

The value of stETH reflects staking rewards via a daily rebase:
```
If you hold 10 stETH and the daily staking reward is 0.01%:
  Day 0: 10.0000 stETH
  Day 1: 10.0010 stETH
  Day 365: ~10.365 stETH (approximately 3.65% APR)
```

**rETH (Rocket Pool) pricing:**

rETH appreciates against ETH rather than rebasing:
```
Exchange rate at launch: 1 rETH = 1.000 ETH
Exchange rate after 1 year at ~4% APR: 1 rETH = 1.040 ETH
```

**Risks of liquid staking:**
- **Smart contract risk** — bugs in the staking contract could lock or lose funds
- **Slashing risk** — validator misbehavior affects stakers' deposits
- **De-peg risk** — LSTs can trade below the value of the underlying ETH during market stress (as stETH did in June 2022)
- **Centralization risk** — Lido's dominance raises concerns about one entity controlling a large share of validator slots

**Source:** Ethereum Foundation. (2024). Staking on Ethereum. https://ethereum.org/en/staking/

---

## 3.8 Layer 2 Scaling Solutions

### 3.8.1 Why Layer 2 Is Needed

Ethereum mainnet (Layer 1) processes approximately 15-30 transactions per second with a target gas limit of 15 million per block. During periods of high demand (NFT mints, DeFi activity, memecoin launches), gas prices spike to hundreds of Gwei, making simple transactions cost $10-$100+ and complex DeFi interactions cost $50-$500+.

> **Definition: Layer 2 (L2)**
>
> A Layer 2 is a separate blockchain that extends Ethereum by inheriting its security guarantees while processing transactions off of the main chain (Layer 1). Layer 2 solutions batch many transactions together and post compressed summaries to Ethereum, achieving higher throughput and lower costs while still relying on Ethereum for final settlement and data availability.

**The scalability trilemma** (coined by Vitalik Buterin) states that a blockchain can optimize for only two of three properties:
1. **Decentralization** — Many independent nodes can participate
2. **Security** — The network is resistant to attacks
3. **Scalability** — The network processes many transactions per second

Layer 2 solutions address this by moving execution off-chain while anchoring security to Layer 1. Ethereum's strategy is to remain optimized for decentralization and security at Layer 1, while delegating scalability to Layer 2 networks.

### 3.8.2 Optimistic Rollups

> **Definition: Optimistic Rollup**
>
> An Optimistic Rollup is a Layer 2 scaling solution that executes transactions off-chain and posts transaction data (or compressed state differences) to Ethereum. It is "optimistic" because transactions are assumed to be valid by default — no validity proofs are computed. Instead, there is a challenge period (typically 7 days) during which anyone can submit a fraud proof to demonstrate that a transaction was invalid. If fraud is proven, the invalid transaction is reverted and the malicious sequencer is penalized.

**How Optimistic Rollups work:**

1. **Sequencer** — A centralized operator (in most current implementations) that collects transactions, orders them, and executes them off-chain
2. **Batch submission** — The sequencer batches transactions and posts the transaction data (or state roots) to an Ethereum smart contract
3. **State commitment** — The sequencer posts the resulting state root to Ethereum
4. **Challenge period** — Anyone can verify the off-chain execution by re-executing the transactions against the posted data. If the result does not match the sequencer's claim, a fraud proof can be submitted
5. **Fraud proof** — If submitted and validated, the invalid state is rolled back and the sequencer loses their bond

**Challenge period tradeoff:**
- The 7-day challenge window is necessary for security — it provides enough time for honest verifiers to detect and prove fraud
- However, it means withdrawals from Layer 2 (L2) to Layer 1 (L1) take 7 days
- Third-party liquidity bridges (e.g., Across, Hop Protocol) offer faster withdrawals by fronting the funds for a fee

**Major Optimistic Rollups:**

| Rollup | Key Features | Transactions per second (TPS) | Notes |
|--------|-------------|-----|-------|
| **Arbitrum** | Nitro architecture, fraud proofs based on WebAssembly (WASM), Stylus (Rust/C++ smart contracts) | ~40,000 (theoretical) | Largest L2 by total value locked (TVL) |
| **Optimism** | OP Stack (modular rollup framework), Bedrock upgrade, revenue sharing | ~2,000 | Powers the Superchain vision |
| **Base** | Built on OP Stack, operated by Coinbase, no native token | ~2,000 | Fastest-growing L2 by users |

### 3.8.3 ZK-Rollups

> **Definition: Zero-Knowledge Rollup (ZK-Rollup)**
>
> A ZK-Rollup is a Layer 2 scaling solution that executes transactions off-chain and generates a cryptographic validity proof (a zero-knowledge proof or ZK proof) that proves the correctness of all transactions in the batch. This proof is verified on Ethereum by a smart contract. Unlike Optimistic Rollups, ZK-Rollups do not require a challenge period — once the proof is verified, the transactions are considered final.

> **Definition: Zero-Knowledge Proof (ZKP)**
>
> A zero-knowledge proof is a cryptographic method by which one party (the prover) can prove to another party (the verifier) that a statement is true without revealing any information beyond the truth of the statement itself. In the context of ZK-Rollups, the prover demonstrates that a batch of transactions was executed correctly without requiring the verifier to re-execute them.

**How ZK-Rollups work:**

1. **Execution** — Transactions are collected and executed off-chain by the sequencer
2. **Proof generation** — A specialized prover generates a validity proof (succinct non-interactive argument of knowledge (SNARK) or scalable transparent argument of knowledge (STARK)) that cryptographically attests to the correctness of all state transitions
3. **On-chain verification** — The proof is submitted to a verifier contract on Ethereum, which checks the proof (verification is cheap: ~200,000-500,000 gas regardless of how many transactions were in the batch)
4. **State update** — Once the proof is verified, the new state root is accepted as valid

**Types of zero-knowledge proofs:**

| Property | Zero-knowledge succinct non-interactive arguments of knowledge (zk-SNARKs) | Zero-knowledge scalable transparent arguments of knowledge (zk-STARKs) |
|----------|-----------|-----------|
| Full name | Zero-Knowledge Succinct Non-interactive Arguments of Knowledge | Zero-Knowledge Scalable Transparent Arguments of Knowledge |
| Proof size | Small (~200 bytes) | Larger (~50-100 KB) |
| Verification time | Fast (constant time) | Fast (polylogarithmic) |
| Trusted setup | Required (vulnerability if compromised) | Not required (transparent) |
| Quantum resistance | No | Yes |
| Prover time | Faster | Slower |
| Used by | zkSync, Polygon zero-knowledge Ethereum Virtual Machine (zkEVM), Scroll | StarkNet |

**Major ZK-Rollups:**

| Rollup | Proof System | EVM Compatibility | Notes |
|--------|-------------|-------------------|-------|
| **zkSync Era** | zk-SNARKs (PLONK) | Custom zkEVM (type 4) | Native account abstraction, compiler-based approach |
| **StarkNet** | zk-STARKs | Non-EVM (Cairo language) | No trusted setup, most mathematically distinct |
| **Polygon zkEVM** | zk-SNARKs | Type 2 zkEVM (high EVM equivalence) | Close to bytecode-level EVM compatibility |
| **Scroll** | zk-SNARKs | Type 2 zkEVM | Bytecode-level EVM equivalence, community-driven |
| **Linea** | zk-SNARKs | Type 2 zkEVM | Developed by Consensys (MetaMask creators) |

### 3.8.4 Optimistic vs ZK Rollups: Comparison

| Property | Optimistic Rollups | ZK-Rollups |
|----------|-------------------|------------|
| **Validity mechanism** | Fraud proofs (reactive) | Validity proofs (proactive) |
| **Withdrawal time to L1** | ~7 days (challenge period) | Minutes to hours (once proof is verified) |
| **Computational cost** | Low (only compute during disputes) | High (generating proofs is expensive) |
| **On-chain data** | Full transaction data or state diffs | State diffs + proof |
| **EVM compatibility** | High (easier to achieve) | Challenging (proving EVM execution in ZK is hard) |
| **Maturity** | More mature (Arbitrum, Optimism since 2021) | Rapidly maturing (mainnet launches 2023-2024) |
| **Gas cost per transaction** | Low | Lowest (amortized proof verification) |
| **Security assumption** | At least one honest verifier during challenge period | Cryptographic soundness of the proof system |
| **Best for** | General-purpose DeFi, compatibility-focused apps | High-throughput applications, fast finality needs |

### 3.8.5 Data Availability and EIP-4844

> **Definition: Data Availability**
>
> Data availability refers to the guarantee that the data needed to verify a blockchain's state transitions is actually published and accessible to network participants. For rollups, this means the transaction data posted to Ethereum must be available long enough for anyone to reconstruct the rollup's state and (for Optimistic Rollups) submit fraud proofs. Without data availability, a malicious sequencer could commit invalid state transitions that no one could challenge.

**The data availability problem:**
Rollups post transaction data to Ethereum calldata, which is stored permanently by all Ethereum nodes. This is the most expensive part of a rollup transaction — calldata costs 16 gas per non-zero byte, and rollup transactions are often dominated by this cost.

**EIP-4844: Proto-Danksharding (activated March 2024, Dencun upgrade)**

> **Definition: EIP-4844 (Proto-Danksharding)**
>
> EIP-4844 introduced a new transaction type called "blob-carrying transactions" that creates a separate, cheaper data space specifically for rollup data. Blobs are large (~128 KB) data attachments that are available on the network for approximately 18 days and then pruned (deleted). This dramatically reduces the cost of posting rollup data to Ethereum without bloating the permanent state.

**Key concepts:**
- **Blob** — A ~128 KB data chunk attached to a transaction. Each block can contain up to 6 blobs (target 3).
- **Blob fee market** — Separate from the regular gas market, with its own EIP-1559-style base fee mechanism
- **Kate-Zaverucha-Goldberg (KZG) commitments** — A polynomial commitment scheme used to verify blob data without downloading entire blobs (enables future data availability sampling)
- **Pruning** — Blob data is available for ~18 days, then deleted. This is sufficient for rollup verification but does not permanently bloat the chain.

**Impact on rollup costs:**
- Pre-EIP-4844: Rollup transactions cost $0.10-$1.00+ on Arbitrum/Optimism
- Post-EIP-4844: Costs dropped to $0.001-$0.01 for many transactions
- This represents a **10x-100x cost reduction** for Layer 2 users

**Source:** Buterin, V. et al. (2022). EIP-4844: Shard Blob Transactions. https://eips.ethereum.org/EIPS/eip-4844

### 3.8.6 The Rollup-Centric Roadmap

In October 2020, Vitalik Buterin articulated Ethereum's shift to a "rollup-centric roadmap" — the recognition that Ethereum's primary scaling strategy would be through Layer 2 rollups rather than increasing Layer 1 capacity directly.

**The vision:**
- **Ethereum Layer 1** serves as the settlement and data availability layer — highly decentralized, secure, and relatively low throughput
- **Layer 2 rollups** handle execution — higher throughput, lower cost, inheriting L1 security
- **Layer 3s** (application-specific chains built on top of L2s) could provide even more specialized scaling

**Full Danksharding (future):**
The complete version of danksharding will dramatically expand data availability:
- 64-128 blobs per block (vs. 6 currently)
- **Data Availability Sampling (DAS)** — Nodes can verify that blob data is available by downloading only a small random sample, rather than the entire blob
- Target: 1-10 MB/s of data throughput, enabling rollups to process thousands of transactions per second at negligible cost

**Source:** Buterin, V. (2020). A rollup-centric ethereum roadmap. https://ethereum-magicians.org/t/a-rollup-centric-ethereum-roadmap/4698

---

## 3.9 The Ethereum Roadmap

Ethereum's development roadmap (as articulated by Vitalik Buterin) is organized into several parallel tracks, each named with a rhyming theme. These tracks are pursued simultaneously rather than sequentially.

### 3.9.1 The Surge (Scaling)

**Goal:** Achieve 100,000+ transactions per second across Layer 1 and Layer 2 combined.

**Key initiatives:**
- **Full Danksharding** — Expanding blob capacity from 6 per block to 64-128, with data availability sampling (DAS) so individual validators do not need to download all data
- **EIP-4844 improvements** — Increasing blob count and reducing costs further
- **Rollup maturity** — Supporting the progression of rollups from centralized sequencers to decentralized, permissionless systems
- **Cross-rollup interoperability** — Standards and infrastructure for seamless asset and message passing between L2s

### 3.9.2 The Scourge (Censorship Resistance and MEV Mitigation)

**Goal:** Ensure censorship resistance and mitigate the negative effects of Maximal Extractable Value (MEV).

> **Definition: Maximal Extractable Value (MEV)**
>
> Maximal Extractable Value (formerly "Miner Extractable Value") is the maximum value that can be extracted from block production beyond the standard block reward and gas fees, by including, excluding, or reordering transactions within a block. MEV includes profits from front-running, sandwich attacks, arbitrage, and liquidations. MEV creates negative externalities for regular users (higher slippage, failed transactions) and centralizing pressures on block production.

**Key initiatives:**
- **Proposer-Builder Separation (PBS)** — Separating the roles of block proposers (validators) and block builders (entities that construct optimally ordered blocks), reducing centralization pressure
- **Inclusion lists** — Mechanisms allowing proposers to guarantee that certain transactions are included, preventing censorship
- **MEV burn** — Returning MEV profits to the protocol (burning or redistributing) rather than concentrating them among block builders
- **Encrypted mempools** — Preventing front-running by encrypting transaction details until they are committed to a block

### 3.9.3 The Verge (Verification Efficiency)

**Goal:** Make block verification so lightweight that any device (including mobile phones and embedded devices) can verify Ethereum.

**Key initiatives:**
- **Verkle Trees** — Replacing Merkle Patricia Tries with Verkle Trees, a more efficient data structure that uses polynomial commitments (Inner Product Arguments or KZG) to produce much smaller proofs
  - Merkle proof size: ~1 KB per account
  - Verkle proof size: ~150 bytes per account
  - Enables stateless clients that can verify blocks without storing the full state
- **Stateless clients** — Nodes that can verify blocks using only the block data and witness proofs (no local state database required)
- **SNARKifying the EVM** — Using zero-knowledge proofs to verify EVM execution, allowing anyone to verify the entire chain by checking a single proof

### 3.9.4 The Purge (State Management)

**Goal:** Reduce the storage and computational burden on nodes by expiring old state and history.

**Key initiatives:**
- **History expiry (EIP-4444)** — Nodes are no longer required to store historical blocks older than ~1 year. Archived data would be available via specialized services (Portal Network) but not required for consensus
- **State expiry** — State that has not been accessed for a long period is "expired" — it still exists but must be revived with a witness proof to be used. This caps the amount of state a node must actively maintain
- **Log/receipt pruning** — Reducing the amount of event/log data that nodes must store

**Why this matters:**
- Ethereum's state is currently ~150+ GB and growing
- Without purging, running a full node becomes increasingly expensive
- Reducing requirements keeps the validator set decentralized

### 3.9.5 The Splurge (Miscellaneous Improvements)

**Goal:** Address everything else — smaller improvements that do not fit neatly into the other categories.

**Key initiatives:**
- **Account abstraction (ERC-4337 and beyond)** — Making all accounts smart contract accounts, enabling social recovery, gas sponsorship, batched transactions, and custom signature schemes
- **EVM improvements** — EVM Object Format (EOF) for better code validation and versioning
- **Cryptographic upgrades** — Boneh-Lynn-Shacham (BLS) signature aggregation, precompiles for new cryptographic primitives
- **Protocol simplification** — Removing technical debt, deprecated opcodes, and unnecessary complexity

**Source:** Buterin, V. (2023). The Ethereum Roadmap. https://ethereum.org/en/roadmap/

---

## Key Takeaways

1. **Ethereum extends blockchain beyond money to programmable applications.** Its Turing-complete virtual machine allows developers to deploy arbitrary logic as smart contracts, enabling DeFi, NFTs, DAOs, and applications that do not yet exist.

2. **The account-based model fundamentally differs from Bitcoin's UTXO model.** Ethereum maintains explicit global state (the world state trie) mapping every address to its balance, nonce, code, and storage, enabling complex stateful applications.

3. **The EVM is sandboxed, deterministic, and gas-metered.** These properties ensure that every node reaches the same result for every transaction, that contracts cannot access external systems (preserving determinism), and that computation is bounded (preventing denial-of-service attacks).

4. **Gas is the unit of computational cost, and EIP-1559 transformed the fee market.** The base fee is algorithmically adjusted and burned, making gas prices more predictable and giving ETH a deflationary mechanism during periods of high network activity.

5. **Smart contract security is paramount because code is immutable and holds real value.** The history of Ethereum is shaped by exploits (The DAO, reentrancy, oracle manipulation), and best practices like Checks-Effects-Interactions, auditing, and formal verification are essential.

6. **Token standards (ERC-20, ERC-721, ERC-1155, ERC-4626) enable composable, interoperable ecosystems.** By standardizing interfaces, any wallet, exchange, or protocol can interact with any compliant token without custom integration.

7. **The Merge transitioned Ethereum from Proof-of-Work to Proof-of-Stake, reducing energy consumption by 99.95%.** Validators now stake 32 ETH and earn rewards for honest block production, with slashing as the penalty for misbehavior.

8. **Layer 2 rollups are Ethereum's primary scaling strategy.** Optimistic Rollups assume validity and use fraud proofs; ZK-Rollups prove validity cryptographically. Both inherit Ethereum's security while offering dramatically lower costs.

9. **EIP-4844 (proto-danksharding) reduced Layer 2 costs by 10-100x** by introducing blob transactions — a cheaper, temporary data space purpose-built for rollup data posting.

10. **Ethereum's roadmap (Surge, Scourge, Verge, Purge, Splurge) addresses scaling, censorship resistance, verification efficiency, state management, and protocol refinement** as parallel tracks, with the ultimate goal of supporting 100,000+ TPS across the L1+L2 ecosystem while remaining maximally decentralized.

---

## Further Reading

### Primary Sources
- Buterin, V. (2013). Ethereum Whitepaper: A Next-Generation Smart Contract and Decentralized Application Platform. https://ethereum.org/en/whitepaper/
- Wood, G. (2014). Ethereum: A Secure Decentralised Generalised Transaction Ledger (Yellow Paper). https://ethereum.github.io/yellowpaper/paper.pdf
- Buterin, V. et al. (2019). EIP-1559: Fee market change for ETH 1.0 chain. https://eips.ethereum.org/EIPS/eip-1559
- Buterin, V. et al. (2022). EIP-4844: Shard Blob Transactions. https://eips.ethereum.org/EIPS/eip-4844

### Books
- Antonopoulos, A. & Wood, G. (2018). Mastering Ethereum: Building Smart Contracts and DApps. O'Reilly Media. https://github.com/ethereumbook/ethereumbook
- Zheng, P. et al. (2020). Blockchain and Smart Contract. Springer.
- Solidity Documentation. https://docs.soliditylang.org/

### Technical Documentation
- Ethereum Developer Documentation. https://ethereum.org/en/developers/docs/
- Ethereum Improvement Proposals (EIPs). https://eips.ethereum.org/
- OpenZeppelin Contracts Documentation. https://docs.openzeppelin.com/contracts/
- Ethereum Consensus Specifications. https://github.com/ethereum/consensus-specs

### Research
- Buterin, V. et al. (2020). Combining GHOST and Casper. https://arxiv.org/abs/2003.03052
- Atzei, N., Bartoletti, M., & Cimoli, T. (2017). A Survey of Attacks on Ethereum Smart Contracts. Principles of Security and Trust (POST). https://eprint.iacr.org/2016/1007
- Daian, P. et al. (2020). Flash Boys 2.0: Frontrunning in Decentralized Exchanges, Miner Extractable Value, and Consensus Instability. IEEE S&P. https://arxiv.org/abs/1904.05234
- Perez, D. & Livshits, B. (2021). Smart Contract Vulnerabilities: Vulnerable Does Not Imply Exploited. USENIX Security. https://arxiv.org/abs/1902.06710

---

## Computational Exercises

The following notebooks provide hands-on implementations of concepts covered in this section:

- **`notebooks/01-cryptographic-primitives.ipynb`** — Keccak-256 hashing (Ethereum's hash function), digital signatures with secp256k1, and Merkle tree implementations directly applicable to understanding Ethereum's state trie and transaction verification.

- **`notebooks/03-ethereum-evm-analysis.ipynb`** (upcoming) — Interact with Ethereum nodes via Web3.py, decode EVM bytecode and opcodes, trace smart contract execution, analyze gas consumption patterns, and query the state trie. Includes exercises on decoding ERC-20 transfer events and simulating EIP-1559 base fee dynamics.

- **`notebooks/04-smart-contract-development.ipynb`** (upcoming) — Write, compile, deploy, and test Solidity smart contracts using Brownie or Foundry. Implement an ERC-20 token from scratch, deploy to a local testnet, and interact with it programmatically. Includes exercises on common vulnerability patterns (reentrancy, access control) and their mitigations.

- **`notebooks/05-defi-protocols.ipynb`** (upcoming) — Analyze DeFi protocols built on Ethereum: simulate Uniswap constant-product automated market maker (AMM) pricing, calculate impermanent loss, model Aave liquidation mechanics, and explore ERC-4626 vault share pricing. Integrates with the token standards covered in Section 3.6.
