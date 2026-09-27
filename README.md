# The Crypto Ecosystem - From Bitcoin to Web 3

## Overview

A comprehensive exploration of the cryptocurrency ecosystem, from the origins of Bitcoin through Ethereum, smart contracts, DeFi, NFTs, and Web 3. Combines theoretical understanding with hands-on computational analysis using Python and Jupyter Lab.

**Based on:** MIT Sloan School of Management - Blockchain and Crypto Applications Course

All content uses open-source resources with full source citations.

---

## Project Structure

```
sections/        # Markdown content (theory, history, analysis)
notebooks/       # Jupyter Lab notebooks (hands-on computation)
environment.yml  # Mamba/Conda environment for all notebooks
CLAUDE.md        # Project configuration and progress tracking
```

---

## Sections (Reading Material)

### 01: Historical Evolution - From Cypherpunks to Web 3
**File:** `sections/01-historical-evolution.md`

Trace the complete history of cryptocurrency from the 1990s Cypherpunk movement through Bitcoin's creation, Ethereum's smart contract revolution, and the modern DeFi/NFT/Web 3 ecosystem.

**Key Topics:** Cypherpunk philosophy, early digital cash attempts, Bitcoin's genesis, Ethereum and smart contracts, DeFi Summer, NFT explosion, Web 3 vision, current state and emerging trends

---

### 02: Bitcoin Deep Dive - Technical Architecture & Economics
**File:** `sections/02-bitcoin-deep-dive.md`

Master Bitcoin's technical architecture including the UTXO model, Proof-of-Work mining, difficulty adjustment, and transaction structure. Explore Bitcoin's economic model with its fixed 21 million supply, halving schedule, and fee markets.

**Key Topics:** Blockchain structure and Merkle trees, UTXO model vs. account-based systems, mining and difficulty adjustment, fixed supply economics and halvings, transaction fees, scalability challenges, Lightning Network

---

### 03: Ethereum & Smart Contracts - Programmable Blockchains
**File:** `sections/03-ethereum-smart-contracts.md`

Dive deep into Ethereum's architecture, the EVM, and Solidity programming. Learn how smart contracts enable decentralized applications, token standards, and DeFi primitives.

**Key Topics:** Ethereum architecture and account model, EVM and gas mechanism, Solidity programming, token standards (ERC-20, ERC-721, ERC-1155), DeFi building blocks, The Merge (PoW to PoS), Layer 2 scaling

---

### 04: Blockchain Economics - Cryptoeconomics & Tokenomics
**File:** `sections/04-blockchain-economics.md`

Understand the economic principles underlying blockchain systems: game theory, incentive design, mechanism design, tokenomics, and valuation frameworks.

**Key Topics:** Cryptoeconomic principles, game theory in consensus protocols, tokenomics (supply curves, inflation/deflation, distribution), staking economics, valuation frameworks (NVT, Stock-to-Flow), market structure

---

### 05: Platform Comparison - Consensus, Trilemma & Scaling
**File:** `sections/05-platform-comparison.md`

Compare blockchain platforms and consensus mechanisms. Analyze the blockchain trilemma trade-offs and Layer 2 scaling approaches.

**Key Topics:** Consensus mechanisms (PoW, PoS, DPoS, BFT), blockchain trilemma, Layer 1 comparison (Ethereum, Solana, Cardano, Avalanche, Polkadot, Cosmos), Layer 2 solutions, modular architecture, performance metrics

---

### 06: Privacy Technologies - Anonymity & Confidentiality
**File:** `sections/06-privacy-technologies.md`

Explore privacy in blockchain systems, blockchain analysis techniques, and privacy-enhancing technologies.

**Key Topics:** Pseudonymity vs. anonymity, blockchain forensics, CoinJoin, ring signatures, stealth addresses, zero-knowledge proofs (zk-SNARKs, zk-STARKs), privacy coins (Monero, Zcash), regulatory challenges

---

### 07: Stablecoins - Price-Stable Cryptocurrencies
**File:** `sections/07-stablecoins.md`

Study stablecoins and their mechanisms for maintaining price stability.

**Key Topics:** Stablecoin categories (fiat-backed, crypto-backed, algorithmic), peg maintenance, use cases, risks and depegging events, case studies (USDC, DAI, Terra/UST collapse)

---

### 08: DAO Governance - Decentralized Organizations
**File:** `sections/08-dao-governance.md`

Understand DAOs and on-chain governance mechanisms.

**Key Topics:** DAO fundamentals, governance mechanisms (token voting, quadratic voting, delegation), DAO frameworks, governance challenges, legal considerations, case studies (MakerDAO, Compound, Uniswap)

---

### 09: Environmental & Sustainability Considerations
**File:** `sections/09-sustainability.md`

Examine the environmental impact of blockchain systems.

**Key Topics:** PoW energy consumption, mining geographic distribution, carbon footprint, e-waste, sustainability initiatives, PoS energy efficiency, renewable energy adoption

---

## Notebooks (Computational)

| # | Notebook | Description |
|---|----------|-------------|
| 01 | `cryptographic-primitives` | Hash functions, ECDSA key pairs, digital signatures, Merkle trees, proof-of-work |
| 02 | `bitcoin-blockchain-analysis` | Node connections, block/transaction parsing, UTXO analysis, network metrics |
| 03 | `ethereum-evm-analysis` | Web3.py node interaction, EVM bytecode/opcode decoding, gas analysis, EIP-1559 simulation |
| 04 | `smart-contract-development` | Solidity ERC-20/721, deployment to testnet, security analysis, Governor contracts |
| 05 | `defi-protocols` | AMM simulators, lending mechanics, liquidation, impermanent loss, flash loans |
| 06 | `market-analysis` | Price data APIs, technical indicators, on-chain metrics, portfolio optimization |
| 07 | `mining-economics` | Profitability calculator, difficulty simulation, pool rewards, 51% attack costs |
| 08 | `valuation-models` | Stock-to-Flow, Metcalfe's Law, NVT ratio, Monte Carlo simulations |
| 09 | `privacy-forensics` | Address clustering, transaction graphs, ZKP basics, mixing detection, taint analysis |
| 10 | `cryptoeconomic-modeling` | Game theory, Nash equilibrium, mechanism design, staking economics, validator yields |
| 11 | `tokenomics` | Supply curves, inflation/deflation, vesting schedules, bonding curves |
| 12 | `governance-simulation` | Quadratic voting, conviction voting, governance attacks, treasury analysis |
| 13 | `stablecoin-analysis` | Peg stability tracking, Terra/UST death spiral simulation, reserve modeling |
| 14 | `consensus-simulations` | PoW/PoS simulators, PBFT, fork choice rules, finality analysis |
| 15 | `multichain-analysis` | Throughput benchmarks, cost analysis, decentralization metrics |
| 16 | `energy-sustainability` | Energy consumption estimation, carbon footprint, PoW vs. PoS comparison |

---

## Setup

### Prerequisites

- Python 3.11+
- Mamba or Conda (Mamba recommended)

### Installation

```bash
# Create environment
mamba env create -f environment.yml

# Activate
conda activate crypto-course

# Launch Jupyter Lab
jupyter lab
```

### API Keys (Optional)

Some notebooks can use external APIs for real-time data. Create a `.env` file in the project root:

```bash
ETHERSCAN_API_KEY=your_key_here
COINGECKO_API_KEY=your_key_here
COINMARKETCAP_API_KEY=your_key_here
INFURA_PROJECT_ID=your_project_id
ALCHEMY_API_KEY=your_key_here
```

Most APIs have generous free tiers. Notebooks also include alternative methods using publicly available data.

### Troubleshooting

**Environment creation fails:**
```bash
mamba update mamba -n base
mamba env create -f environment.yml
```

**Jupyter kernel not found:**
```bash
conda activate crypto-course
python -m ipykernel install --user --name crypto-course --display-name "Python (Blockchain)"
```

---

## Learning Path

### Recommended Sequence (12 weeks, 6-8 hours/week)

| Weeks | Sections | Notebooks |
|-------|----------|-----------|
| 1-2 | 01: Historical Evolution | 01: Cryptographic Primitives |
| 3-4 | 02: Bitcoin Deep Dive | 02, 07, 08: Bitcoin Analysis, Mining, Valuation |
| 5-6 | 03: Ethereum & Smart Contracts | 03, 04, 05: EVM, Smart Contracts, DeFi |
| 7-8 | 04: Blockchain Economics | 06, 10, 11: Market Analysis, Cryptoeconomics, Tokenomics |
| 9-10 | 05: Platforms, 06: Privacy | 09, 14, 15: Privacy, Consensus, Multi-Chain |
| 11-12 | 07: Stablecoins, 08: DAOs, 09: Sustainability | 12, 13, 16: Governance, Stablecoins, Energy |

### Alternative Paths

- **Fast Track (6 weeks):** Sections 1-4 + Notebooks 01-06
- **DeFi Focused:** Sections 01, 03, 04, 07 + Notebooks 04, 05, 06, 11, 13
- **Technical Deep Dive:** Sections 02, 03, 05, 06 + Notebooks 01, 02, 03, 09, 14
- **Economics Focused:** Sections 01, 04, 07, 09 + Notebooks 06, 07, 08, 10, 11, 16

---

## Resources

### Primary Sources
- **Bitcoin Whitepaper:** https://bitcoin.org/bitcoin.pdf
- **Ethereum Whitepaper:** https://ethereum.org/en/whitepaper/
- **Ethereum Yellowpaper:** https://ethereum.github.io/yellowpaper/paper.pdf

### Open Source Books
- **Mastering Bitcoin:** https://github.com/bitcoinbook/bitcoinbook
- **Mastering Ethereum:** https://github.com/ethereumbook/ethereumbook

### Developer Documentation
- **Bitcoin Developer Guide:** https://developer.bitcoin.org/
- **Ethereum Developer Docs:** https://ethereum.org/en/developers/docs/
- **Solidity Documentation:** https://docs.soliditylang.org/

### Research & Analysis
- **Nakamoto Institute:** https://nakamotoinstitute.org/
- **Messari Research:** https://messari.io/research
- **Coin Metrics:** https://coinmetrics.io/
- **Glassnode Academy:** https://academy.glassnode.com/

---

## License

This educational content is provided under the MIT License for open educational use.

## Acknowledgments

Designed to replicate and expand upon MIT Sloan School of Management's "Blockchain and Crypto Applications: From Decentralized Finance to Web 3" course using entirely open-source materials.
