# Section 7: Stablecoins - Price-Stable Cryptocurrencies

## Table of Contents

- [7.1 The Need for Price Stability](#71-the-need-for-price-stability)
- [7.2 Fiat-Collateralized Stablecoins](#72-fiat-collateralized-stablecoins)
- [7.3 Crypto-Collateralized Stablecoins](#73-crypto-collateralized-stablecoins)
- [7.4 Algorithmic Stablecoins](#74-algorithmic-stablecoins)
- [7.5 Case Study: The Terra/UST Collapse](#75-case-study-the-terraust-collapse)
- [7.6 Stablecoin Peg Mechanisms](#76-stablecoin-peg-mechanisms)
- [7.7 Stablecoins in DeFi](#77-stablecoins-in-defi)
- [7.8 Regulatory Landscape](#78-regulatory-landscape)
- [7.9 Central Bank Digital Currencies (CBDCs)](#79-central-bank-digital-currencies-cbdcs)
- [Key Takeaways](#key-takeaways)
- [Further Reading](#further-reading)
- [Computational Exercises](#computational-exercises)

---

## 7.1 The Need for Price Stability

### 7.1.1 Cryptocurrency Volatility as a Barrier to Adoption

> **Definition: Stablecoin**
>
> A stablecoin is a cryptocurrency designed to maintain a stable value relative to a reference asset, most commonly a fiat currency such as the U.S. dollar. Stablecoins achieve price stability through various mechanisms including fiat reserves, crypto-collateral over-backing, or algorithmic supply adjustments.

Bitcoin, Ether, and most other cryptocurrencies exhibit extreme price volatility. Bitcoin has experienced drawdowns exceeding 70% on multiple occasions (2014, 2018, 2022), and daily price swings of 5-10% are routine. This volatility undermines three of the core functions that money must serve:

| Function of Money | Impact of Volatility |
|---|---|
| **Medium of exchange** | Merchants cannot price goods reliably; buyers hesitate to spend an appreciating asset |
| **Unit of account** | Contract values denominated in crypto fluctuate unpredictably |
| **Store of value** | Holders face the risk of sudden, severe purchasing-power loss |

For cryptocurrency to move beyond speculation and serve as genuine financial infrastructure, a price-stable layer is necessary.

### 7.1.2 Use Cases Requiring Stability

Stablecoins address concrete use cases where volatility is unacceptable:

- **Payments and commerce:** Merchants need assurance that the $100 they receive today will still be worth approximately $100 tomorrow. Stablecoins enable crypto-native payments without exchange-rate risk.
- **Lending and borrowing:** Decentralized Finance (DeFi) lending protocols require stable units for loan denomination. A loan issued in ether (ETH) could double or halve in value before repayment, creating asymmetric risk for borrowers or lenders.
- **Remittances:** Cross-border money transfers through traditional channels (Western Union, Society for Worldwide Interbank Financial Telecommunication (SWIFT)) cost 5-7% on average. Stablecoins on public blockchains can reduce this to fractions of a percent while settling in minutes rather than days.
- **Payroll:** Companies operating across borders use stablecoins to pay contractors in jurisdictions with limited banking infrastructure.
- **Trading pairs:** On cryptocurrency exchanges, stablecoins serve as the quote currency in trading pairs (e.g., bitcoin (BTC) against Tether (USDT)), replacing the need for fiat on-ramps.

### 7.1.3 Stablecoins as the Bridge Between Crypto and Traditional Finance

Stablecoins occupy a unique position at the intersection of traditional finance and the crypto ecosystem. They denominate value in familiar fiat terms while operating on blockchain infrastructure, offering the programmability and composability of crypto with the price stability of the dollar.

This bridging function has driven explosive growth. Stablecoins have become the most heavily transacted category of digital assets, frequently exceeding the combined transaction volume of Visa and Mastercard on a settlement basis.

### 7.1.4 Market Size and Growth

The stablecoin market has grown from negligible value in 2017 to a multi-hundred-billion-dollar asset class:

| Year | Total Stablecoin Market Cap | Key Milestone |
|---|---|---|
| 2017 | ~$1 billion | Tether dominance, limited to crypto trading |
| 2019 | ~$5 billion | USD Coin (USDC) launched by Circle/Coinbase (Centre consortium) |
| 2020 | ~$25 billion | DeFi Summer drives stablecoin demand |
| 2021 | ~$140 billion | Institutional adoption accelerates; TerraUSD (UST) grows rapidly |
| 2022 (pre-crash) | ~$180 billion | Peak market cap before Terra collapse |
| 2022 (post-crash) | ~$135 billion | ~$45 billion wiped out from UST/LUNA collapse |
| 2023 | ~$130 billion | Recovery begins; USDC depegs briefly during Silicon Valley Bank (SVB) crisis |
| 2024 | ~$170 billion | Regulatory clarity drives renewed growth |
| 2025 | ~$210 billion+ | Stablecoin legislation advances in the U.S. and European Union (EU) |

Stablecoin transaction volume tells an even more dramatic story. In 2024, stablecoins settled over $10 trillion in on-chain transactions, rivaling the throughput of major traditional payment networks.

**Source:** DefiLlama. (2025). Stablecoins Dashboard. https://defillama.com/stablecoins

**Source:** Castle Island Ventures & Brevan Howard Digital. (2024). Stablecoins: The Emerging Market Story. https://castleisland.vc/stablecoins-the-emerging-market-story/

---

## 7.2 Fiat-Collateralized Stablecoins

### 7.2.1 How Fiat-Collateralized Stablecoins Work

> **Definition: Fiat-Collateralized Stablecoin**
>
> A fiat-collateralized stablecoin is a digital token backed 1:1 (or greater) by reserves of fiat currency or fiat-equivalent assets (such as Treasury bills or bank deposits) held by a centralized custodian. For every stablecoin token in circulation, the issuer promises to hold the equivalent value in reserves, redeemable upon request.

The mechanism is straightforward:

1. **Minting:** A user deposits $1,000 US dollar (USD) with the issuer (e.g., Circle, Tether). The issuer mints 1,000 stablecoin tokens and sends them to the user's blockchain address.
2. **Circulation:** The tokens circulate freely on public blockchains. Anyone can send, receive, or trade them without the issuer's involvement.
3. **Redemption:** A user sends 1,000 stablecoin tokens back to the issuer. The issuer burns (destroys) the tokens and wires $1,000 USD to the user's bank account.
4. **Peg maintenance:** If the market price deviates from $1.00, arbitrageurs profit by minting or redeeming, which naturally restores the peg (see Section 7.6).

### 7.2.2 USDT (Tether)

> **Definition: USDT (Tether)**
>
> USDT is the first and largest stablecoin by market capitalization, issued by Tether Limited. Originally launched on Bitcoin's Omni Layer in 2014, it now circulates on Ethereum, Tron, Solana, and numerous other blockchains.

**History and growth:**
Tether was founded in 2014 as "Realcoin" by Brock Pierce, Reeve Collins, and Craig Sellars, and rebranded to Tether shortly after. It became the dominant stablecoin largely because it solved an immediate problem for crypto exchanges: providing a dollar-denominated trading pair without requiring traditional banking relationships, which many offshore exchanges could not obtain.

By 2025, USDT's market capitalization exceeded $140 billion, making it the third-largest cryptocurrency overall and commanding over 60% of the stablecoin market.

**Controversies and reserve composition debates:**
Tether has faced persistent questions about whether its reserves fully back all outstanding tokens:

- **2017-2018:** Tether claimed 1:1 USD backing but refused to provide a formal audit. The New York Attorney General (NYAG) launched an investigation.
- **2019:** Tether's terms of service were quietly changed from "backed by USD" to "backed by reserves," which could include loans and other assets.
- **2021 (NYAG settlement):** The NYAG found that Tether had, at various times, lacked full backing. Tether and its affiliate Bitfinex paid an $18.5 million fine without admitting wrongdoing. Tether was required to publish quarterly reserve reports.
- **2021 reserve breakdown (Q1):** Tether's first detailed reserve report revealed that only 3.87% of reserves were in cash, with 49.6% in unspecified commercial paper, raising concerns about credit risk and liquidity.
- **2022-2024:** Tether shifted reserves heavily toward U.S. Treasury bills, and by 2024 reported over 80% of reserves in Treasuries and Treasury repos, significantly strengthening its reserve profile.

**Source:** Office of the Attorney General, State of New York. (2021). Attorney General James Ends Virtual Currency Trading Platform Bitfinex's Illegal Activities in New York. https://ag.ny.gov/press-release/2021/attorney-general-james-ends-virtual-currency-trading-platform-bitfinexs-illegal

| Reserve Category (2024 Q4) | Approximate Share |
|---|---|
| U.S. Treasury bills | ~80% |
| Overnight reverse repos | ~10% |
| Bitcoin, Gold, Other | ~5% |
| Corporate bonds, Secured loans | ~5% |

Despite the controversies, USDT has maintained its peg through multiple market crises and has never failed to honor a redemption request at par, though it briefly traded below $0.95 on some exchanges during market panics.

### 7.2.3 USDC (Circle)

> **Definition: USDC (USD Coin)**
>
> USDC is a fiat-collateralized stablecoin issued by Circle Internet Financial. Originally launched in 2018 as a joint project of Circle and Coinbase under the Centre Consortium, USDC emphasizes regulatory compliance and reserve transparency.

USDC took a deliberately different approach from Tether:

- **Regulated entity:** Circle is a licensed money transmitter in the U.S. and holds an Electronic Money Institution (EMI) license in the EU.
- **Reserve transparency:** USDC reserves are held in segregated accounts at regulated financial institutions and invested in short-duration U.S. Treasuries. Circle publishes monthly attestation reports from Deloitte (previously Grant Thornton).
- **Reserve composition:** Circle maintains reserves almost entirely in cash and short-term U.S. Treasuries through the Circle Reserve Fund (managed by BlackRock), providing high transparency and liquidity.
- **Compliance features:** USDC smart contracts include a "blacklist" function that allows Circle to freeze tokens at specific addresses, a feature used in response to law enforcement requests and sanctions enforcement.

USDC reached a peak market cap of approximately $55 billion in mid-2022 before declining to around $25 billion after the Silicon Valley Bank (SVB) episode (see Section 7.6) and competitive pressure from USDT. By 2025, USDC had recovered to approximately $35-40 billion.

**Source:** Circle. (2025). USDC Transparency and Attestation Reports. https://www.circle.com/en/transparency

### 7.2.4 BUSD: A Regulatory Shutdown

Binance USD (BUSD) was a fiat-collateralized stablecoin issued by Paxos Trust Company and branded by Binance, the world's largest crypto exchange. At its peak in late 2022, BUSD had a market cap of approximately $23 billion.

In February 2023, the New York Department of Financial Services (NYDFS) ordered Paxos to stop minting new BUSD tokens, citing "several unresolved issues related to Paxos's oversight of its relationship with Binance." The Securities and Exchange Commission (SEC) also issued Paxos a Wells notice, suggesting it might classify BUSD as an unregistered security.

Paxos complied immediately, and BUSD's market cap declined steadily as tokens were redeemed and not replaced. By early 2024, BUSD had effectively wound down to near zero. The episode demonstrated that fiat-collateralized stablecoins are ultimately subject to the regulatory authority of the jurisdictions where their issuers operate.

**Source:** NYDFS. (2023). DFS Orders Paxos to Cease Minting Binance-Branded Stablecoin. https://www.dfs.ny.gov/

### 7.2.5 Advantages of Fiat-Collateralized Stablecoins

- **Simplicity:** The 1:1 backing model is easy to understand and communicate.
- **Strong peg maintenance:** The direct redeemability for fiat creates a hard floor and ceiling for arbitrageurs.
- **Capital efficiency:** Each dollar of collateral supports one dollar of stablecoin (no overcollateralization required).
- **Proven track record:** USDT and USDC have maintained their pegs through extreme market conditions, including the March 2020 crash, the May 2022 Terra collapse, and the March 2023 banking crisis.

### 7.2.6 Risks of Fiat-Collateralized Stablecoins

- **Counterparty risk:** Users must trust the issuer to hold adequate reserves and honor redemptions. If the issuer becomes insolvent or is fraudulent, tokens become worthless.
- **Regulatory risk:** Issuers operate within legal jurisdictions and can be shut down by regulators (as with BUSD). New legislation could impose requirements that alter stablecoin economics.
- **Centralization:** A single entity controls minting, burning, and (often) freezing of tokens. This contradicts the decentralization ethos of cryptocurrency.
- **Censorship and freezing:** Both Tether and Circle have frozen tokens at specific addresses in response to law enforcement requests. As of 2024, Tether had frozen over $1 billion in USDT across hundreds of addresses. This demonstrates that fiat-collateralized stablecoins are not censorship-resistant.
- **Banking dependency:** Issuers require banking relationships to hold reserves and process redemptions. The collapse of crypto-friendly banks (Silvergate, Silicon Valley Bank, Signature Bank) in early 2023 underscored this dependency.

### 7.2.7 Reserve Composition and Transparency Requirements

The quality of a stablecoin's reserves directly determines its safety. Key considerations include:

| Factor | Safer | Riskier |
|---|---|---|
| Asset type | Cash, T-bills | Commercial paper, corporate bonds, crypto |
| Liquidity | Overnight, on-demand | Locked, long-duration |
| Custody | Segregated, regulated | Commingled, offshore |
| Verification | Full audit by Big Four firm | Attestation, self-reported |
| Jurisdiction | U.S., EU (strong rule of law) | Offshore (BVI, Cayman) |

The distinction between an **audit** and an **attestation** is important. An audit is a comprehensive examination of financial statements and internal controls. An attestation is a narrower engagement where an accounting firm verifies specific claims (e.g., "reserves exceeded liabilities on this particular date"). Most stablecoin issuers provide attestations, not audits. No major stablecoin issuer has yet completed a full public audit by a Big Four accounting firm.

---

## 7.3 Crypto-Collateralized Stablecoins

### 7.3.1 How Crypto-Collateralized Stablecoins Work

> **Definition: Crypto-Collateralized Stablecoin**
>
> A crypto-collateralized stablecoin is a digital token whose value is backed by cryptocurrency assets locked in smart contracts rather than by fiat reserves held by a centralized entity. Because the collateral itself is volatile, these stablecoins require overcollateralization: the locked crypto must be worth significantly more than the stablecoins minted against it.

> **Definition: Overcollateralization**
>
> Overcollateralization is the practice of providing collateral whose value exceeds the value of the asset being issued or borrowed. In crypto-collateralized stablecoin systems, overcollateralization provides a safety margin against the price decline of the collateral. For example, a 150% overcollateralization ratio means that $150 worth of crypto must be locked to mint $100 of stablecoins.

The basic flow:

1. A user deposits cryptocurrency (e.g., ETH) into a smart contract (called a "vault" or "collateralized debt position (CDP)").
2. The smart contract allows the user to mint stablecoins up to a maximum determined by the collateralization ratio.
3. To retrieve the locked crypto, the user must repay the minted stablecoins plus any accrued stability fees.
4. If the value of the locked collateral falls below a threshold, the position is liquidated to protect the system's solvency.

### 7.3.2 MakerDAO and DAI Deep Dive

> **Definition: MakerDAO**
>
> MakerDAO is a decentralized autonomous organization on Ethereum that governs the Maker Protocol, which issues DAI, the largest decentralized stablecoin. Founded by Rune Christensen in 2014, MakerDAO pioneered the concept of crypto-collateralized stablecoins and Collateralized Debt Positions. In 2024, MakerDAO rebranded to Sky, with DAI transitioning to USDS (though DAI remains widely used).

**Collateralized Debt Positions (CDPs) / Vaults**

> **Definition: Collateralized Debt Position (CDP)**
>
> A Collateralized Debt Position (renamed "Vault" in Multi-Collateral DAI) is a smart contract that holds a user's locked collateral and tracks the amount of DAI minted against it. The vault enforces the collateralization ratio and triggers liquidation if the ratio falls below the required minimum.

**How a Maker Vault works — numerical example:**

Suppose Alice wants to generate DAI using ETH as collateral, with ETH priced at $2,000 and a minimum collateralization ratio of 150%.

1. **Deposit:** Alice deposits 10 ETH into a Maker Vault.
   - Collateral value: 10 ETH x $2,000 = **$20,000**
2. **Mint DAI:** At 150% minimum collateralization, Alice can mint up to:
   - Maximum DAI = $20,000 / 1.50 = **13,333 DAI**
   - Alice conservatively mints **10,000 DAI** (200% collateralization ratio)
3. **Use DAI:** Alice can use the 10,000 DAI for trading, lending, or any on-chain activity.
4. **Close vault:** To retrieve her 10 ETH, Alice must return 10,000 DAI plus the accrued stability fee.

**Collateralization ratio = (Collateral Value / DAI Debt) x 100%**

Alice's ratio: ($20,000 / $10,000) x 100% = **200%**

**Liquidation mechanism — numerical example:**

Continuing Alice's example (10 ETH, 10,000 DAI debt, 150% liquidation ratio):

| ETH Price | Collateral Value | Collateralization Ratio | Status |
|---|---|---|---|
| $2,000 | $20,000 | 200% | Safe |
| $1,800 | $18,000 | 180% | Safe |
| $1,600 | $16,000 | 160% | Warning zone |
| $1,500 | $15,000 | 150% | At liquidation threshold |
| $1,400 | $14,000 | 140% | **Liquidated** |

When ETH drops to $1,500 (or just below), Alice's vault is eligible for liquidation:

1. A **keeper** (automated bot) detects the undercollateralized vault.
2. The keeper triggers the liquidation function on the Maker smart contract.
3. Alice's 10 ETH collateral is auctioned off.
4. From the auction proceeds, the system recovers the 10,000 DAI debt plus a **13% liquidation penalty** (1,300 DAI).
5. Any remaining collateral is returned to Alice.
6. The recovered DAI is burned, reducing DAI supply.

If ETH is at $1,400 at the time of auction:
- Collateral auctioned: 10 ETH x $1,400 = $14,000
- Debt to recover: 10,000 DAI
- Liquidation penalty: 1,300 DAI
- Total system recovery: 11,300 DAI
- Returned to Alice: $14,000 - $11,300 = **$2,700 worth of ETH (~1.93 ETH)**
- Alice lost approximately **$7,300** compared to simply holding 10 ETH ($14,000) without opening the vault.

**Stability Fee and DAI Savings Rate (DSR)**

> **Definition: Stability Fee**
>
> The stability fee is the annual interest rate charged on DAI minted from Maker Vaults. It functions like a borrowing rate and is a primary mechanism for controlling DAI supply. Raising the stability fee makes it more expensive to mint DAI, reducing supply and pushing the price up toward $1.00. Lowering it has the opposite effect.

> **Definition: DAI Savings Rate (DSR)**
>
> The DAI Savings Rate is the interest rate earned by depositing DAI into the DSR smart contract. It incentivizes users to hold DAI rather than sell it, supporting the peg from the demand side. The DSR is funded by stability fee revenue.

The stability fee and DSR work together as monetary policy levers:

| DAI Trading Price | Action | Mechanism |
|---|---|---|
| Below $1.00 (excess supply) | Raise stability fee, raise DSR | Higher borrowing cost reduces minting; higher savings rate increases demand |
| Above $1.00 (excess demand) | Lower stability fee, lower DSR | Cheaper to mint increases supply; lower savings rate reduces hoarding |

**Multi-Collateral DAI (MCD)**

The original Single-Collateral DAI (SCD), launched in December 2017, accepted only ETH as collateral. Multi-Collateral DAI (MCD), launched in November 2019, expanded the system to accept multiple collateral types:

- **Crypto assets:** ETH, Wrapped Bitcoin (WBTC), Chainlink (LINK), Uniswap (UNI), and many others
- **Real-World Assets (RWAs):** U.S. Treasuries (through entities like BlockTower), tokenized short-term bonds
- **Stablecoins:** USDC was controversially added as collateral, effectively making a portion of DAI's backing centralized

By 2024, over 50% of DAI's collateral was composed of Real-World Assets (primarily U.S. Treasuries), reflecting a strategic shift by MakerDAO governance to generate yield and diversify beyond volatile crypto assets.

**Governance via MKR Token**

> **Definition: MKR Token**
>
> Maker (MKR) is the governance token of the Maker Protocol. MKR holders vote on critical protocol parameters including collateral types, stability fees, liquidation ratios, and system upgrades. MKR also serves as the "lender of last resort": if the system becomes undercollateralized (e.g., during a black swan crash), new MKR is minted and auctioned to recapitalize the system, diluting existing MKR holders.

Key governance decisions made by MKR holders:
- Which assets to accept as collateral and their risk parameters
- Stability fee and DSR rates
- Liquidation ratios and penalties
- Emergency shutdown procedures
- Protocol upgrades and integrations

**Source:** MakerDAO. (2020). The Maker Protocol: MakerDAO's Multi-Collateral Dai (MCD) System. https://makerdao.com/en/whitepaper/

### 7.3.3 Liquity and LUSD

Liquity is an alternative decentralized stablecoin protocol launched in April 2021 that takes a more purist approach to decentralization than MakerDAO:

- **Governance-free:** No governance token and no voting. All protocol parameters are fixed and immutable at deployment.
- **Immutable contracts:** The smart contracts cannot be upgraded, eliminating governance attack risk.
- **ETH-only collateral:** Only accepts ETH, avoiding exposure to other tokens' risks.
- **Minimum collateral ratio:** 110% (significantly lower than Maker's 150%), enabling greater capital efficiency.
- **One-time borrowing fee:** Instead of ongoing stability fees, Liquity charges a one-time, algorithmically determined borrowing fee.
- **Stability pool:** Liquity USD (LUSD) holders can deposit into a stability pool that automatically purchases discounted collateral from liquidated positions, providing a return to depositors while ensuring efficient liquidations.

Liquity's design trades governance flexibility for immutability and censorship resistance. Its LUSD stablecoin has maintained a strong peg despite having no governance mechanism to adjust parameters.

**Source:** Kolchinsky, R. (2021). Liquity: Decentralized Borrowing Protocol. https://docs.liquity.org/

### 7.3.4 Advantages of Crypto-Collateralized Stablecoins

- **Decentralized:** No single entity controls minting, burning, or freezing. The system operates through open smart contracts.
- **Transparent:** All collateral is visible on-chain in real time. Anyone can verify that the system is adequately collateralized.
- **Censorship-resistant:** No entity can freeze or blacklist specific tokens (in truly immutable implementations like Liquity).
- **Composable:** Deeply integrated into the DeFi ecosystem as a building block for lending, trading, and yield protocols.

### 7.3.5 Risks of Crypto-Collateralized Stablecoins

- **Liquidation cascades:** In sharp market downturns, many vaults breach their liquidation thresholds simultaneously. Mass liquidations flood the market with sell pressure on collateral assets, driving prices lower, triggering more liquidations in a feedback loop. On "Black Thursday" (March 12, 2020), ETH dropped 43% in a single day, causing $8.3 million in Maker vaults to be liquidated at near-zero prices due to network congestion.
- **Governance attacks:** In governance-based systems like MakerDAO, an attacker who accumulates enough governance tokens could vote to change protocol parameters maliciously (e.g., accepting worthless tokens as collateral to drain the system).
- **Oracle dependency:** These systems rely on price oracles (data feeds) to determine collateral values. If an oracle is manipulated or fails, liquidations may trigger incorrectly, or the system may become undercollateralized without triggering any liquidations.
- **Capital inefficiency:** Overcollateralization means that the user must lock up $150 or more to generate $100 of stablecoins, tying up capital unproductively.
- **Smart contract risk:** Bugs in the underlying smart contracts could lead to loss of collateral or unauthorized minting.

### 7.3.6 Mathematical Example: Opening a Vault and Liquidation Threshold

**Problem:** Bob wants to open a Maker Vault using WBTC (Wrapped Bitcoin) as collateral. The current BTC price is $60,000, the minimum collateralization ratio is 150%, and the liquidation penalty is 13%.

**Step 1: Deposit and mint**
- Bob deposits 1 WBTC ($60,000 in value)
- Maximum DAI he can mint: $60,000 / 1.50 = 40,000 DAI
- Bob mints 30,000 DAI (conservative, giving him a 200% collateralization ratio)

**Step 2: Calculate liquidation price**
- Liquidation occurs when: Collateral Value / Debt = Minimum Collateralization Ratio
- Liquidation price = (DAI Debt x Min. Ratio) / Amount of Collateral
- Liquidation price = (30,000 x 1.50) / 1 = **$45,000 per BTC**
- If BTC falls from $60,000 to $45,000 (a 25% drop), Bob's vault will be liquidated.

**Step 3: Liquidation outcome (if BTC = $44,000)**
- Collateral value: 1 WBTC x $44,000 = $44,000
- Debt: 30,000 DAI
- Liquidation penalty: 30,000 x 13% = 3,900 DAI
- System recovers: 33,900 DAI
- Returned to Bob: $44,000 - $33,900 = **$10,100 in WBTC (~0.23 WBTC)**
- Bob's loss relative to simply holding: He kept 30,000 DAI + received ~$10,100 = ~$40,100 total versus $44,000 from holding. Net loss from the vault: **~$3,900** (the liquidation penalty).

> **Notebook Reference:** See `notebooks/05-defi-protocols.ipynb` (upcoming) for interactive vault simulations, liquidation modeling, and collateralization ratio analysis.

---

## 7.4 Algorithmic Stablecoins

### 7.4.1 How Algorithmic Stablecoins Work

> **Definition: Algorithmic Stablecoin**
>
> An algorithmic stablecoin attempts to maintain its peg to a target price (typically $1.00) through automated supply expansion and contraction mechanisms, typically without holding any (or sufficient) external collateral. Instead of backing, these systems rely on market incentives and arbitrage to maintain price stability.

Algorithmic stablecoins represent the most ambitious and most fragile category of stablecoins. They attempt to solve a fundamental challenge: maintaining a fixed price for a free-floating token using only software and incentive design.

### 7.4.2 Seigniorage Shares Model

> **Definition: Seigniorage**
>
> Seigniorage is the profit made by a government or issuer from issuing currency, historically the difference between the face value of money and its production cost. In algorithmic stablecoin systems, seigniorage refers to the profit generated when new stablecoins are minted during periods of excess demand.

The seigniorage shares model, first proposed by Robert Sams in 2014, uses a two-token system:

- **Stablecoin token:** The price-stable token pegged to $1.00
- **Share/governance token:** Absorbs volatility and captures seigniorage profits

**When the stablecoin trades above $1.00 (excess demand):**
1. The protocol mints new stablecoins to increase supply
2. New stablecoins are distributed to share token holders as seigniorage profit
3. Increased supply pushes the stablecoin price back toward $1.00

**When the stablecoin trades below $1.00 (excess supply):**
1. The protocol offers "bonds" or "coupons" at a discount (e.g., buy at $0.80, redeemable for $1.00 when the peg recovers)
2. Users burn stablecoins to purchase bonds, reducing supply
3. Reduced supply pushes the stablecoin price back toward $1.00

The critical weakness: the contraction mechanism relies on faith that the peg will eventually recover. If confidence collapses, no one buys the bonds, supply cannot contract, and the peg breaks permanently.

**Source:** Sams, R. (2014). A Note on Cryptocurrency Stabilisation: Seigniorage Shares. https://github.com/rmsams/stablecoins

### 7.4.3 Rebase Mechanisms

> **Definition: Rebase**
>
> A rebase is an automatic, protocol-level adjustment of all token holders' balances to increase or decrease total supply. In a rebase stablecoin, if the price is above the peg, every wallet's balance increases proportionally. If the price is below, every wallet's balance decreases. The token price adjusts, but each holder's share of total supply remains constant.

Ampleforth (AMPL) pioneered this approach. Every 24 hours, the protocol checks the volume-weighted average price (VWAP) of AMPL:
- If AMPL > $1.05: All wallets receive additional AMPL proportionally (positive rebase)
- If AMPL < $0.95: All wallets have AMPL removed proportionally (negative rebase)
- If $0.95 < AMPL < $1.05: No rebase (equilibrium zone)

**Example:** If you hold 1,000 AMPL and the price is $1.20, a rebase might increase your balance to 1,200 AMPL. Your total dollar value remains approximately $1,200, but now you have more tokens each worth (theoretically) closer to $1.00.

The challenge with rebase mechanisms is that negative rebases are psychologically punishing to holders, triggering sell pressure that drives further rebases downward.

### 7.4.4 Fractional Algorithmic Designs: FRAX

FRAX, created by Sam Kazemian, introduced the concept of a fractional algorithmic stablecoin. Rather than being fully collateralized or fully algorithmic, FRAX operated on a spectrum:

- A portion of each FRAX is backed by collateral (initially USDC)
- The remaining portion is backed algorithmically via the FRAX Share (FXS) token
- The **Collateral Ratio (CR)** adjusts dynamically based on market conditions

**Example at 85% CR:**
- To mint 100 FRAX, a user provides $85 in USDC and $15 worth of FXS
- The FXS is burned, reducing its supply
- If demand for FRAX increases, the CR decreases (system needs less collateral)
- If demand for FRAX decreases, the CR increases (system requires more collateral for safety)

In practice, FRAX eventually moved to 100% collateralization (fully backed by stablecoins and real-world assets) after the Terra/UST collapse demonstrated the fragility of algorithmic mechanisms. This evolution is itself a lesson about the limitations of purely algorithmic designs.

**Source:** Kazemian, S. et al. (2020). FRAX: Fractional-Algorithmic Stablecoin Protocol. https://docs.frax.finance/

### 7.4.5 The Fundamental Challenge: Reflexivity and the "Death Spiral"

> **Definition: Reflexivity (in Stablecoin Context)**
>
> Reflexivity refers to a self-reinforcing feedback loop where falling confidence in a stablecoin causes selling, which drives the price further below the peg, which further erodes confidence, creating a vicious cycle. In algorithmic stablecoins, reflexivity can lead to a "death spiral" where the contraction mechanism fails to reduce supply fast enough to restore the peg, and the companion token's value collapses simultaneously.

The fundamental problem with algorithmic stablecoins is that they work when confidence is high but fail precisely when they are needed most. During a crisis:

1. The stablecoin falls below $1.00
2. The protocol tries to contract supply by offering incentives (bonds, share tokens)
3. The incentive tokens themselves are losing value because they depend on the system's credibility
4. Rational actors refuse to "buy the dip" because the expected value of recovery is declining
5. Supply cannot contract, the stablecoin falls further, and the death spiral accelerates

This reflexivity problem is why nearly every purely algorithmic stablecoin has eventually failed to maintain its peg: Basis Cash, Empty Set Dollar, Iron Finance (which experienced a bank-run-style collapse in June 2021), and most catastrophically, Terra's UST.

---

## 7.5 Case Study: The Terra/UST Collapse

### 7.5.1 How Terra/UST Worked

> **Definition: Terra/UST**
>
> TerraUSD (UST) was an algorithmic stablecoin on the Terra blockchain that maintained its peg through a mint/burn mechanism with LUNA, the Terra network's native token. Created by Do Kwon and Terraform Labs, Terra/UST grew to become the third-largest stablecoin by market cap before its catastrophic collapse in May 2022.

**The mint/burn mechanism:**

The Terra protocol maintained UST's peg through an arbitrage relationship with LUNA:

- **To mint 1 UST:** Burn $1 worth of LUNA. The protocol always values $1 worth of LUNA regardless of UST's market price.
- **To redeem 1 UST:** Burn 1 UST and receive $1 worth of LUNA.

**Peg maintenance through arbitrage:**

| Scenario | Arbitrage Action | Effect on Supply |
|---|---|---|
| UST > $1.00 | Burn $1 of LUNA, mint 1 UST, sell UST for >$1.00 on market | Increases UST supply, pushing price down |
| UST < $1.00 | Buy UST for <$1.00 on market, redeem for $1.00 of LUNA | Decreases UST supply, pushing price up |

**Numerical example (during normal operation):**
- UST is trading at $1.02 on exchanges
- LUNA is trading at $80
- Arbitrageur burns 0.0125 LUNA ($1.00 worth), mints 1 UST
- Sells 1 UST on the market for $1.02
- Profit: $0.02 per UST (minus gas fees)
- This selling pressure brings UST back toward $1.00

The critical design flaw: when UST is redeemed, the protocol mints LUNA. If there is a large rush to exit UST, massive amounts of LUNA are created, diluting its value, which reduces confidence in the system's ability to honor further redemptions.

### 7.5.2 Anchor Protocol and the 20% Yield

> **Definition: Anchor Protocol**
>
> Anchor Protocol was a lending and borrowing platform on the Terra blockchain that offered approximately 19.5-20% Annual Percentage Yield (APY) on UST deposits. This yield was subsidized by Terraform Labs and borrower collateral yields, not generated organically from market demand. Anchor became the primary source of demand for UST, holding over 70% of all UST in circulation at its peak.

The 20% yield on Anchor was the engine driving UST's growth:

1. UST market cap grew from ~$2 billion in January 2022 to ~$18.7 billion by May 2022
2. Approximately $14 billion of UST was deposited in Anchor
3. The yield was not sustainable — Anchor's "yield reserve" (a subsidy fund) was depleting rapidly, dropping from $450 million to under $200 million by May 2022
4. The yield attracted users who treated UST as a high-yield savings account, not understanding the risk that the peg could break

The unsustainable yield created a dangerous dynamic: most UST demand was not organic (people using UST for transactions) but was entirely dependent on the continuation of the 20% subsidy. If the yield dropped, users would withdraw, creating massive sell pressure on UST.

### 7.5.3 The Depeg Sequence: May 7-13, 2022

**Saturday, May 7:**
- Large UST withdrawals from Anchor: approximately $2 billion removed in 48 hours
- Large UST sell orders appear on Curve Finance (the primary stablecoin exchange), with over $350 million in UST sold
- UST begins trading slightly below peg at $0.985
- Terraform Labs' Luna Foundation Guard (LFG) begins deploying Bitcoin reserves (approximately $1.5 billion in BTC) to defend the peg

**Sunday, May 8:**
- UST drops to $0.95
- Panic accelerates as users rush to withdraw from Anchor
- LFG deploys more Bitcoin, selling BTC to buy UST
- The BTC selling adds downward pressure to the broader crypto market

**Monday, May 9:**
- UST crashes to $0.67
- LUNA drops from $60 to $30 as massive minting occurs to absorb UST redemptions
- The mint/burn mechanism is now actively destroying LUNA's value
- Crypto Twitter erupts; Do Kwon tweets reassurances

**Tuesday, May 10:**
- UST briefly recovers to $0.93 before falling again to $0.30
- LUNA collapses from $30 to $5
- The death spiral is now fully engaged: UST redemptions mint billions of LUNA, LUNA's price crashes, each UST redemption requires more LUNA to be minted, further crashing its price

**Wednesday, May 11:**
- UST falls to $0.15
- LUNA's supply has hyperinflated from 350 million to over 6.5 billion tokens
- LUNA price: $0.10 (from $80 just four days earlier)

**Thursday, May 12-Friday, May 13:**
- Terra blockchain is halted twice to prevent governance attacks
- LUNA supply exceeds 6.5 trillion tokens
- UST stabilizes at approximately $0.10; LUNA effectively reaches $0
- Terraform Labs announces "Terra 2.0" (a fork without UST), widely seen as an inadequate response

### 7.5.4 The Death Spiral Mechanics

The Terra collapse followed a textbook death spiral with self-reinforcing feedback loops:

```
UST sells below $1.00
        |
        v
Arbitrageurs burn UST for LUNA
        |
        v
Massive LUNA minting (supply hyperinflation)
        |
        v
LUNA price crashes
        |
        v
Each UST redemption requires more LUNA to be minted
        |
        v
Confidence in LUNA (the backing) collapses
        |
        v
Remaining UST holders rush to exit
        |
        v
More UST sells below $1.00 (cycle repeats, accelerating)
```

The core problem was that UST's "backing" was LUNA, but LUNA's value derived from the Terra ecosystem, which derived its value from UST adoption. This circular dependency meant that stress in either token immediately propagated to the other.

### 7.5.5 Scale of Destruction

| Metric | Before Collapse | After Collapse |
|---|---|---|
| UST market cap | ~$18.7 billion | ~$1.8 billion (at $0.10) |
| LUNA market cap | ~$28 billion | ~$0 |
| LUNA price | ~$80 | < $0.0001 |
| LUNA supply | ~350 million | ~6.5 trillion |
| Anchor total value locked (TVL) | ~$14 billion | $0 |
| Total value destroyed | | **~$40 billion** |

The collapse affected thousands of retail investors, many of whom had concentrated their savings in Anchor's 20% yield. Reports of personal financial devastation and suicides followed.

### 7.5.6 Lessons Learned

1. **Endogenous collateral is not collateral.** If a stablecoin is "backed" by a companion token whose value depends on the stablecoin's success, the backing is circular and illusory.
2. **Unsustainable yields are a warning sign.** The 20% APY on Anchor could not be generated organically and was subsidized to attract deposits. When the subsidy ran out, so did the demand.
3. **Algorithmic stablecoins face reflexivity risk.** The contraction mechanism (minting LUNA to absorb UST selling) amplified the problem rather than solving it.
4. **Size does not equal safety.** UST was the third-largest stablecoin with an $18.7 billion market cap. Scale provided no protection against a fundamentally flawed design.
5. **Bitcoin reserves were insufficient.** LFG's $1.5 billion in Bitcoin reserves were far too small relative to UST's $18.7 billion market cap and were spent in days.

### 7.5.7 Impact on the Broader Crypto Market

The Terra collapse triggered contagion across the crypto ecosystem:

- Bitcoin fell from $36,000 to $26,000 in the week following the collapse (partly due to LFG's forced BTC selling)
- Total crypto market cap declined by approximately $400 billion
- Three Arrows Capital (3AC), a major crypto hedge fund with UST/LUNA exposure, became insolvent, triggering the collapse of Voyager Digital and Celsius Network
- The cascade of failures contributed to the conditions that led to FTX's collapse in November 2022
- Regulators worldwide accelerated stablecoin legislation efforts

**Source:** Liu, J., Makarov, I., & Schoar, A. (2023). Anatomy of a Run: The Terra Luna Crash. National Bureau of Economic Research Working Paper 31160. https://www.nber.org/papers/w31160

**Source:** Briola, A. et al. (2023). Anatomy of a Stablecoin's Failure: The Terra-Luna Case. Finance Research Letters, 51. https://doi.org/10.1016/j.frl.2022.103358

---

## 7.6 Stablecoin Peg Mechanisms

### 7.6.1 Arbitrage as the Primary Peg Maintenance Mechanism

> **Definition: Arbitrage**
>
> Arbitrage is the practice of profiting from price differences for the same asset in different markets. In stablecoin systems, arbitrageurs maintain the peg by buying stablecoins when they trade below $1.00 (and redeeming them at par from the issuer) or selling newly minted stablecoins when they trade above $1.00. This activity generates profit for the arbitrageur while pushing the price back toward $1.00.

Arbitrage is the most important force maintaining stablecoin pegs. It works because there is a reliable mechanism to convert between the stablecoin and its underlying value (the "primary market"), while the stablecoin also trades freely on exchanges (the "secondary market").

**Example: USDC trading at $0.98 on an exchange**

1. Arbitrageur buys 100,000 USDC on the exchange for $98,000
2. Arbitrageur redeems 100,000 USDC with Circle for $100,000
3. Profit: $2,000 (minus fees)
4. The buying pressure on the exchange pushes USDC back toward $1.00

**Example: USDC trading at $1.02 on an exchange**

1. Arbitrageur deposits $100,000 with Circle and mints 100,000 USDC
2. Arbitrageur sells 100,000 USDC on the exchange for $102,000
3. Profit: $2,000 (minus fees)
4. The selling pressure on the exchange pushes USDC back toward $1.00

The tighter the arbitrage loop (faster redemption, lower fees, lower minimums), the tighter the peg.

### 7.6.2 Primary Market vs. Secondary Market

| Feature | Primary Market | Secondary Market |
|---|---|---|
| **Participants** | Authorized institutions, large users | Anyone (traders, users, bots) |
| **Mechanism** | Direct mint/redeem with issuer | Exchange trading (decentralized exchange (DEX) or centralized exchange (CEX)) |
| **Price** | Always $1.00 (by definition) | Fluctuates based on supply/demand |
| **Settlement time** | Hours to days (requires bank wires) | Seconds to minutes |
| **Minimum size** | Often $100,000+ (USDC commercial) | Any amount |

The lag between primary and secondary markets creates temporary deviations from the peg, but as long as the primary market functions reliably, the peg will recover.

### 7.6.3 Redemption Guarantees and Their Importance

The strength of a stablecoin's peg is directly proportional to the credibility of its redemption guarantee:

- **Strong guarantee (USDC):** Regulated issuer, transparent reserves in T-bills, attestation reports, licensed in multiple jurisdictions. Arbitrageurs have high confidence in redemption.
- **Moderate guarantee (USDT):** Profitable issuer with large reserves, but offshore jurisdiction (BVI), no formal audit, and historical transparency issues. Arbitrageurs factor in a small counterparty risk premium.
- **Weak guarantee (algorithmic):** No collateral backing; "redemption" means receiving a volatile companion token. Arbitrageurs are only willing to participate if they believe the system will survive.

### 7.6.4 Case Study: USDC's SVB Depeg (March 2023)

On Friday, March 10, 2023, Silicon Valley Bank (SVB) was shut down by the Federal Deposit Insurance Corporation (FDIC). Circle disclosed that approximately $3.3 billion of USDC's ~$43 billion in reserves were held at SVB.

**Timeline:**

| Date | Event | USDC Price |
|---|---|---|
| March 10 (Fri) | SVB shut down; Circle discloses $3.3 billion exposure | $0.98 |
| March 11 (Sat) | No traditional banking to process redemptions; panic selling | $0.87 |
| March 12 (Sun) | U.S. government announces all SVB depositors will be made whole | $0.96 |
| March 13 (Mon) | Banking reopens; Circle confirms full access to SVB funds | $1.00 |

**Key observations:**

1. **The depeg was rational:** $3.3B of $43B reserves (7.7%) were at risk. The market priced USDC at $0.87, implying worse-than-worst-case losses.
2. **The primary market was closed:** Banks do not process wires on weekends. The arbitrage loop was broken because no one could redeem USDC for dollars.
3. **DeFi cascading effects:** The USDC depeg caused DAI to depeg (since significant DAI collateral was USDC). Curve pools became severely imbalanced. The 3pool (USDT/USDC/DAI) saw USDC concentration exceed 90%.
4. **Recovery was swift:** Once the government backstopped SVB deposits and banking reopened Monday, arbitrageurs restored the peg within hours.

The SVB episode demonstrated that even well-managed fiat stablecoins carry banking system risk and that weekend/holiday periods create peg vulnerability windows when the primary market is unavailable.

**Source:** Circle. (2023). An Update on USDC and Silicon Valley Bank. https://www.circle.com/blog/an-update-on-usdc-and-silicon-valley-bank

### 7.6.5 Peg Stability Analysis Framework

When evaluating a stablecoin's peg robustness, consider these dimensions:

| Dimension | Strong Peg | Weak Peg |
|---|---|---|
| **Collateral quality** | Cash, T-bills | Volatile crypto, endogenous tokens |
| **Redemption speed** | Same-day or T+1 | Days, or dependent on market conditions |
| **Redemption access** | Open to all participants | Restricted to whitelisted entities |
| **Market depth** | Deep liquidity on multiple exchanges | Thin order books, few venues |
| **Arbitrageur participation** | Many independent arbitrageurs | Few participants, concentrated risk |
| **Track record** | Maintained peg through crises | Depegged during stress events |
| **Legal enforceability** | Regulated entity with legal obligations | No legal claim to redemption |

---

## 7.7 Stablecoins in DeFi

### 7.7.1 Stablecoins as the Unit of Account in DeFi

Stablecoins are the foundational building block of Decentralized Finance (DeFi). While DeFi protocols can technically operate with any token, stablecoins are overwhelmingly preferred because:

- **Pricing:** DeFi users think in dollar terms. A lending rate of "5% APY on USDC" is immediately comprehensible; "5% APY on ETH" obscures the dollar-denominated return.
- **Risk isolation:** Using stablecoins separates protocol risk (smart contract bugs, oracle failures) from price risk (asset volatility). A user lending USDC only faces protocol risk, not ETH price risk.
- **Composability:** Stablecoins serve as a neutral intermediary between different DeFi protocols. A user can borrow USDC from Aave, deposit it in a Curve pool, stake the liquidity provider (LP) token in Convex, and later repay the loan — all denominated in a stable unit.

As of 2025, stablecoins represent over $50 billion in Total Value Locked (TVL) across DeFi protocols, accounting for roughly one-third of all DeFi TVL.

### 7.7.2 Liquidity Pairs and Automated Market Maker (AMM) Pools

> **Definition: Automated Market Maker (AMM)**
>
> An Automated Market Maker is a decentralized exchange mechanism that uses a mathematical formula to price assets in a liquidity pool, rather than relying on order books. The most common formula is the constant product formula (x * y = k), used by Uniswap. Liquidity providers deposit pairs of tokens, and traders swap against the pool, with prices adjusting automatically based on the ratio of tokens in the pool.

Stablecoin pairs are among the most heavily traded on Decentralized Exchanges (DEXs):

- **Stablecoin-to-crypto pairs:** ETH/USDC, WBTC/USDT — enable trading volatile assets against a stable quote currency
- **Stablecoin-to-stablecoin pairs:** USDC/USDT, DAI/USDC — enable low-slippage conversion between stablecoins
- **Stablecoin liquidity provision:** Providing liquidity to stablecoin pairs carries minimal impermanent loss (since both tokens target $1.00), making it an attractive yield strategy

### 7.7.3 Lending and Borrowing

Stablecoins are the most borrowed and lent assets in DeFi lending protocols (Aave, Compound, Spark):

**Typical lending flow:**
1. Borrower deposits ETH as collateral in Aave
2. Borrows USDC against the collateral (e.g., at 75% Loan-to-Value ratio)
3. Uses USDC for other investments or expenses
4. Repays USDC plus interest to reclaim ETH collateral

**Numerical example:**
- Alice deposits 10 ETH ($20,000) into Aave
- Borrows 12,000 USDC (60% loan-to-value (LTV)) at 4% APY
- After one year, she owes 12,480 USDC
- If ETH has risen to $3,000, Alice repays 12,480 USDC, reclaims 10 ETH ($30,000), and has effectively leveraged her ETH exposure

Stablecoin borrowing rates serve as a benchmark interest rate for the DeFi ecosystem, analogous to the federal funds rate in traditional finance. When demand for stablecoin borrowing is high (bull market leverage), rates can exceed 10-15%. During low-demand periods, rates may fall below 2%.

### 7.7.4 Yield Generation Strategies

Stablecoin holders can earn yield through several DeFi mechanisms:

| Strategy | Typical APY Range | Risk Level | Description |
|---|---|---|---|
| Lending (Aave, Compound) | 2-8% | Low-Medium | Deposit stablecoins; earn interest from borrowers |
| Liquidity provision (Curve) | 3-12% | Medium | Provide liquidity to stablecoin pools; earn trading fees + Curve DAO Token (CRV) rewards |
| DAI Savings Rate (DSR) | 5-15% | Low | Deposit DAI in the DSR contract; earn yield funded by stability fees |
| Yield aggregators (Yearn) | 4-15% | Medium | Automated strategies that optimize across protocols |
| Real-world asset vaults | 4-8% | Medium | Stablecoins deployed to fund tokenized T-bill or corporate debt positions |

These yields are generated from real economic activity (borrower interest, trading fees, T-bill yields), unlike Anchor's subsidized 20% APY.

### 7.7.5 Curve Finance and the "Curve Wars"

> **Definition: Curve Finance**
>
> Curve Finance is a Decentralized Exchange (DEX) optimized for stablecoin-to-stablecoin swaps using a specialized bonding curve (the StableSwap invariant) that concentrates liquidity near the peg price. This design enables very large stablecoin trades with minimal slippage — often less than 0.01% on multi-million dollar swaps.

Curve became the most strategically important protocol in DeFi because controlling Curve liquidity for a stablecoin directly supports that stablecoin's peg stability. Deeper Curve liquidity means larger trades can occur without moving the price, making the stablecoin more useful and more trusted.

**The Curve Wars:**

CRV, Curve's governance token, controls the allocation of CRV emissions (reward tokens) to different pools. Directing CRV emissions to your stablecoin's pool incentivizes liquidity providers to deposit there, deepening liquidity.

The competition to control CRV emissions became known as the "Curve Wars":

1. **Convex Finance** accumulated voting power by collecting CRV from depositors, becoming the largest CRV voter
2. **Stablecoin projects** (FRAX, UST, Magic Internet Money (MIM), and others) spent millions acquiring Convex voting tokens (CVX) to direct CRV emissions toward their pools
3. **Bribing protocols** (Votium, Hidden Hand) emerged, allowing projects to pay Convex/Curve voters directly for their votes

The Curve Wars demonstrated that stablecoin peg stability is not just a technical problem — it is also a liquidity competition. The stablecoin with the deepest Curve pool enjoys tighter peg maintenance and greater usability.

**Source:** Egorov, M. (2019). StableSwap - Efficient Mechanism for Stablecoin Liquidity. https://curve.fi/stableswap-paper.pdf

### 7.7.6 Stablecoin Swap Protocols and Concentrated Liquidity

Beyond Curve, several innovations have improved stablecoin trading efficiency:

- **Uniswap v3 concentrated liquidity:** Liquidity providers can concentrate their capital within a tight price range (e.g., $0.999 to $1.001 for stablecoin pairs), dramatically improving capital efficiency for stablecoin swaps.
- **Aggregators (1inch, Paraswap):** Route large stablecoin trades across multiple DEXs and pools to minimize slippage.
- **Request for Quote (RFQ) systems:** Protocols like Hashflow enable market makers to provide quotes for large stablecoin trades off-chain, settling on-chain.

---

## 7.8 Regulatory Landscape

### 7.8.1 Markets in Crypto-Assets (MiCA) Regulation in the EU

> **Definition: Markets in Crypto-Assets (MiCA)**
>
> MiCA is a comprehensive regulatory framework adopted by the European Union in 2023 and fully effective from December 30, 2024. It establishes rules for crypto-asset issuers and service providers across all EU member states, including specific provisions for stablecoins (which MiCA categorizes as Electronic Money Tokens and Asset-Referenced Tokens).

MiCA's stablecoin provisions include:

- **Electronic Money Tokens (EMTs):** Stablecoins pegged to a single fiat currency (e.g., USDC pegged to USD) must be issued by authorized credit institutions or electronic money institutions.
- **Asset-Referenced Tokens (ARTs):** Stablecoins pegged to a basket of assets or multiple currencies face additional requirements including minimum capital, reserve management rules, and limits on daily transaction volume for "significant" tokens.
- **Reserve requirements:** Issuers must maintain reserves in custody at credit institutions, with at least 30% of reserves held in bank deposits (60% for "significant" tokens).
- **Redemption rights:** Token holders must have the right to redeem at par at any time.
- **Volume caps:** "Significant" stablecoins face daily transaction volume limits of 200 million euros, designed to prevent stablecoins from displacing sovereign currencies.

MiCA's impact has been significant: Tether chose not to apply for an EMT license in the EU, and several EU exchanges delisted USDT for European users. Circle obtained an EMT license, positioning USDC as the compliant option in Europe.

**Source:** European Parliament. (2023). Regulation (EU) 2023/1114 on Markets in Crypto-Assets (MiCA). https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32023R1114

### 7.8.2 U.S. Stablecoin Legislation Efforts

The United States has pursued stablecoin-specific legislation through several attempts:

- **The Stablecoin TRUST Act (2022):** Proposed requiring stablecoin issuers to be insured depository institutions or licensed money transmitters with 100% reserves.
- **The Clarity for Payment Stablecoins Act (2023):** Passed the House Financial Services Committee, proposing federal and state licensing pathways for stablecoin issuers with reserve and disclosure requirements.
- **The GENIUS Act (2025):** The Guiding and Establishing National Innovation for U.S. Stablecoins Act advanced through the Senate, proposing a dual federal-state framework requiring 1:1 reserve backing in high-quality liquid assets, monthly attestations, and consumer protection provisions.

As of early 2026, the U.S. has not yet enacted comprehensive stablecoin legislation, though multiple bills remain under active consideration. The regulatory ambiguity has pushed some issuers to seek more favorable jurisdictions while simultaneously driving industry lobbying for clear rules.

### 7.8.3 Reserve Requirements and Auditing Standards

Emerging regulatory standards globally are converging on several key requirements:

| Requirement | Description |
|---|---|
| **1:1 reserve backing** | Reserves must equal or exceed outstanding token supply |
| **Eligible reserve assets** | Cash, Treasury bills, high-quality liquid assets (HQLAs) — no crypto, corporate debt, or risky assets |
| **Segregation** | Reserves must be held in segregated accounts, separate from the issuer's operating funds |
| **Regular reporting** | Monthly or quarterly attestation reports from independent accounting firms |
| **Redemption rights** | Token holders must be able to redeem at face value within a reasonable timeframe (e.g., 1-2 business days) |
| **Capital requirements** | Issuers must maintain minimum capital buffers beyond the 1:1 reserve |

### 7.8.4 Bank-Like Regulation Proposals

Several regulators and policymakers have argued that stablecoin issuers are functionally equivalent to narrow banks (banks that hold only safe, liquid assets) and should be regulated accordingly:

- **Federal Reserve:** A 2022 Fed paper suggested that stablecoins could be regulated as "synthetic central bank money" if issuers were required to hold reserves exclusively at the Fed.
- **Office of the Comptroller of the Currency (OCC):** Has issued interpretive letters affirming that national banks may hold stablecoin reserves and that stablecoin activity is within the "business of banking."
- **Bank of England:** Proposed that systemic stablecoins should be regulated to the same standard as commercial bank deposits, with equivalent consumer protection.

The tension: bank-like regulation provides safety and legitimacy but imposes compliance costs and barriers to entry that favor large, established financial institutions over crypto-native startups.

### 7.8.5 Offshore vs. Onshore Issuers

The regulatory landscape has created a split between onshore and offshore stablecoin issuers:

| Feature | Onshore (e.g., Circle/USDC) | Offshore (e.g., Tether/USDT) |
|---|---|---|
| **Jurisdiction** | U.S., EU | British Virgin Islands |
| **Licensing** | State money transmitter, EMI (EU) | BVI business company |
| **Transparency** | Monthly attestations, SEC oversight | Quarterly reports, limited oversight |
| **Censorship** | Can freeze tokens per law enforcement | Can freeze tokens at discretion |
| **Market access** | Compliant with major jurisdictions | May face delistings (e.g., EU under MiCA) |
| **Market share** | ~25% of stablecoin market | ~65% of stablecoin market |

Tether's offshore structure has been simultaneously its vulnerability (regulatory uncertainty, transparency concerns) and its strength (flexibility, global reach, no single regulatory chokepoint).

### 7.8.6 Impact of Regulation on Stablecoin Design and Adoption

Regulation is shaping stablecoin design in several ways:

1. **Move toward full collateralization:** Post-Terra, both regulators and the market strongly prefer fully collateralized designs. Algorithmic stablecoins face de facto regulatory prohibitions in many jurisdictions.
2. **Compliance features built in:** New stablecoins are being designed with compliance features (know your customer (KYC) and anti-money laundering (AML) hooks, freeze functions, transfer restrictions) from inception.
3. **Institutional adoption:** Regulatory clarity is enabling banks and traditional financial institutions to integrate stablecoins into payment, settlement, and treasury management workflows.
4. **Global coordination challenges:** Different regulatory approaches across jurisdictions create arbitrage opportunities and compliance complexity for global issuers.

---

## 7.9 Central Bank Digital Currencies (CBDCs)

### 7.9.1 Definition and Distinction from Stablecoins

> **Definition: Central Bank Digital Currency (CBDC)**
>
> A Central Bank Digital Currency is a digital form of a country's sovereign currency, issued and backed directly by the central bank. Unlike stablecoins, which are private-sector liabilities backed by reserves, a CBDC is a direct liability of the central bank itself — it carries the same credit risk as physical cash (essentially zero in a country's own currency). CBDCs are sometimes described as "digital cash."

Key distinctions between CBDCs and stablecoins:

| Feature | CBDC | Private Stablecoin |
|---|---|---|
| **Issuer** | Central bank (government) | Private company |
| **Liability of** | Sovereign state | Private issuer |
| **Credit risk** | Sovereign risk (effectively zero in own currency) | Counterparty risk of issuer |
| **Monetary policy** | Direct instrument of the central bank | Subject to monetary policy indirectly |
| **Privacy** | Determined by government policy | Determined by issuer + blockchain design |
| **Permissioning** | Government-controlled access | Varies (permissionless to fully KYC'd) |
| **Programmability** | Limited in most designs | High (smart contract composability) |

As of 2025, over 130 countries (representing 98% of global gross domestic product (GDP)) were exploring CBDCs in some form, according to the Atlantic Council's CBDC tracker.

**Source:** Atlantic Council. (2025). Central Bank Digital Currency Tracker. https://www.atlanticcouncil.org/cbdctracker/

### 7.9.2 CBDC Architectures

> **Definition: Wholesale CBDC**
>
> A wholesale CBDC is restricted to financial institutions (banks, payment service providers) for interbank settlement and large-value transactions. It replaces or supplements existing central bank reserve systems. End consumers do not interact with wholesale CBDCs directly.

> **Definition: Retail CBDC**
>
> A retail CBDC is available to the general public, functioning as a digital equivalent of physical cash. Individuals and businesses can hold and transact with retail CBDCs directly, without requiring a commercial bank account.

CBDCs can be further classified by how balances are tracked:

| Architecture | Description | Privacy | Example |
|---|---|---|---|
| **Account-based** | Users hold accounts at the central bank or intermediaries; transactions update balances in a central ledger | Lower — central bank can see all transactions | Most current CBDC pilots |
| **Token-based** | Digital tokens are issued and transferred peer-to-peer, similar to physical cash; possession equals ownership | Higher — transactions can be anonymous below certain thresholds | Some research prototypes |
| **Hybrid/Intermediated** | Central bank issues the CBDC but commercial banks and payment providers manage user-facing accounts and transactions | Moderate — intermediaries handle KYC; central bank has access | China's e-CNY, EU digital euro proposal |

### 7.9.3 China's Digital Yuan (e-CNY)

China's e-CNY (also called Digital Currency Electronic Payment, or DCEP) is the world's most advanced large-economy CBDC pilot:

- **Launch:** Pilot began in 2020 in four cities; expanded to over 25 cities by 2023
- **Architecture:** Two-tier system — the People's Bank of China (PBOC) issues e-CNY to commercial banks, which distribute it to the public
- **Scale:** By late 2024, cumulative e-CNY transactions exceeded 7 trillion yuan (~$1 trillion), with over 260 million individual wallets opened
- **Features:** Supports offline transactions via Near-Field Communication (NFC), programmable payments (e.g., government subsidies restricted to specific merchants), and "controlled anonymity" for small transactions
- **Adoption challenges:** Despite government promotion, voluntary adoption has been modest. Most usage occurs during government-subsidized promotions (e.g., digital red envelopes, transit subsidies)

**Source:** People's Bank of China. (2021). Progress of Research and Development of E-CNY in China. http://www.pbc.gov.cn/en/

### 7.9.4 European Central Bank Digital Euro Project

The European Central Bank (ECB) has been developing a digital euro:

- **Investigation phase:** Launched in October 2021
- **Preparation phase:** Began in November 2023, expected to run through 2025
- **Proposed design:** Intermediated model where commercial banks distribute digital euros; the ECB operates the back-end infrastructure
- **Holding limits:** The ECB has discussed a holding limit of 3,000 euros per person to prevent bank disintermediation (users moving deposits from commercial banks to the central bank)
- **Privacy model:** Small, in-person transactions would have "cash-like" privacy; larger or online transactions would be subject to standard Anti-Money Laundering (AML) checks
- **Legislative status:** The European Commission proposed a regulation in June 2023 establishing the legal framework; legislative process ongoing

**Source:** European Central Bank. (2023). A Stocktake on the Digital Euro. https://www.ecb.europa.eu/paym/digital_euro/

### 7.9.5 Federal Reserve and the Digital Dollar

The United States has taken a more cautious approach to CBDCs:

- **Research:** The Federal Reserve Bank of Boston partnered with MIT's Digital Currency Initiative on "Project Hamilton" (2020-2022), which demonstrated that a CBDC could process 1.7 million transactions per second on a single node.
- **FedNow:** Launched in July 2023, FedNow is a real-time payment system (not a CBDC) that enables instant bank-to-bank transfers 24/7. While not a digital currency, FedNow addresses some of the same use cases that a retail CBDC would serve (instant payments, financial inclusion).
- **Political resistance:** Several bills have been introduced in Congress to prohibit the Federal Reserve from issuing a retail CBDC, reflecting concerns about financial surveillance and government overreach. The political environment has generally been hostile to a U.S. CBDC.
- **Digital dollar research:** Academic and private-sector research continues through the Digital Dollar Project (led by former Commodity Futures Trading Commission (CFTC) Chairman Christopher Giancarlo) and other initiatives.

**Source:** Federal Reserve Bank of Boston & MIT Digital Currency Initiative. (2022). Project Hamilton Phase 1: A High Performance Payment Processing System. https://www.bostonfed.org/publications/one-time-pubs/project-hamilton-phase-1-executive-summary.aspx

### 7.9.6 Privacy Concerns and Surveillance Implications

CBDCs raise significant privacy and civil liberties concerns that distinguish them from both physical cash and private stablecoins:

- **Transaction surveillance:** A CBDC potentially gives the government complete visibility into every transaction made by every citizen. This capability far exceeds what exists today, where cash provides anonymity and bank records require legal process to access.
- **Programmable restrictions:** CBDCs could be programmed to restrict spending (e.g., government benefits that can only be spent on approved categories), implement negative interest rates (penalizing saving), or expire after a certain date (forcing spending).
- **Financial exclusion as punishment:** Governments could freeze or restrict CBDC access for individuals or groups, effectively cutting them off from the economy without due process.
- **Chilling effects:** Knowledge that every transaction is recorded and potentially monitored may alter behavior — discouraging donations to controversial causes, purchases of sensitive items, or support for political opposition.

Proponents argue that privacy protections can be built into CBDC designs (e.g., anonymity for small transactions, privacy-preserving cryptographic techniques) and that most digital payments already lack anonymity. The debate remains unresolved and is likely to be one of the defining policy issues of the CBDC era.

### 7.9.7 CBDCs vs. Private Stablecoins: Competition or Coexistence?

The relationship between CBDCs and private stablecoins will likely be shaped by several factors:

**Competition scenario:**
- CBDCs could displace stablecoins by offering a risk-free digital dollar with government backing
- Regulators might restrict stablecoins to prevent "monetary fragmentation"
- Banks may prefer CBDCs because they integrate with existing regulatory frameworks

**Coexistence scenario:**
- Stablecoins operate on permissionless blockchains with full DeFi composability; CBDCs likely will not
- Stablecoins serve global, cross-border use cases; CBDCs are national
- Different use cases: CBDCs for domestic payments and government programs; stablecoins for DeFi, cross-border transfers, and crypto-native applications
- The market may support both, much as cash, credit cards, and PayPal coexist today

The most likely outcome, at least in the medium term, is coexistence with differentiated use cases: CBDCs for domestic government-controlled payments and stablecoins for the global, programmable, crypto-native economy.

---

## Key Takeaways

1. **Stablecoins solve the volatility problem** that prevents cryptocurrencies from serving as effective media of exchange, units of account, and stores of value. They are the critical bridge between traditional finance and the crypto ecosystem.

2. **Fiat-collateralized stablecoins (USDT, USDC) dominate the market** because the 1:1 reserve-backed model is simple, intuitive, and creates strong arbitrage-driven peg maintenance. However, they reintroduce centralization and counterparty risk.

3. **Crypto-collateralized stablecoins (DAI) offer decentralization** at the cost of capital efficiency. Overcollateralization protects against collateral volatility, but liquidation cascades during sharp downturns remain a systemic risk.

4. **Algorithmic stablecoins are inherently fragile.** The Terra/UST collapse in May 2022 destroyed approximately $40 billion in value and demonstrated that endogenous collateral creates circular dependencies vulnerable to reflexive death spirals.

5. **Arbitrage is the primary mechanism that maintains stablecoin pegs.** The strength of a peg depends on the speed, cost, and reliability of the redemption mechanism that connects the primary market (issuer) to the secondary market (exchanges).

6. **Stablecoins are the foundation of DeFi,** serving as the unit of account for lending, borrowing, trading, and yield generation. Control of stablecoin liquidity (as demonstrated by the Curve Wars) is strategically critical.

7. **Regulation is rapidly evolving** with MiCA in the EU and pending legislation in the U.S. The trend is toward requiring 1:1 reserve backing, transparent reporting, and issuer licensing. Algorithmic designs face de facto prohibition in many jurisdictions.

8. **CBDCs and stablecoins serve different purposes** and are likely to coexist. CBDCs offer sovereign-backed safety for domestic payments, while stablecoins provide global, programmable, and composable digital dollars for the crypto-native economy.

9. **Reserve quality and transparency determine stablecoin safety.** The distinction between audits and attestations, the composition of reserves (T-bills vs. commercial paper), and the regulatory jurisdiction of the issuer all materially affect risk.

10. **The stablecoin market has demonstrated resilience** through multiple crises (Terra collapse, SVB banking crisis, regulatory shutdowns), with well-collateralized stablecoins recovering quickly while undercollateralized designs failed permanently.

---

## Further Reading

### Primary Sources and Whitepapers
- MakerDAO. (2020). The Maker Protocol: MakerDAO's Multi-Collateral Dai (MCD) System. https://makerdao.com/en/whitepaper/
- Egorov, M. (2019). StableSwap - Efficient Mechanism for Stablecoin Liquidity. https://curve.fi/stableswap-paper.pdf
- Sams, R. (2014). A Note on Cryptocurrency Stabilisation: Seigniorage Shares. https://github.com/rmsams/stablecoins
- Kazemian, S. et al. (2020). FRAX: Fractional-Algorithmic Stablecoin Protocol. https://docs.frax.finance/

### Academic Research
- Liu, J., Makarov, I., & Schoar, A. (2023). Anatomy of a Run: The Terra Luna Crash. NBER Working Paper 31160. https://www.nber.org/papers/w31160
- Briola, A. et al. (2023). Anatomy of a Stablecoin's Failure: The Terra-Luna Case. Finance Research Letters, 51. https://doi.org/10.1016/j.frl.2022.103358
- Lyons, R. & Viswanath-Natraj, G. (2023). What Keeps Stablecoins Stable? Journal of International Money and Finance, 131. https://doi.org/10.1016/j.jimonfin.2022.102777
- Gensler, G. & Bailey, A. (2020). Blockchain and Money. MIT OpenCourseWare 15.S12. https://ocw.mit.edu/courses/15-s12-blockchain-and-money-fall-2018/

### Regulatory Documents
- European Parliament. (2023). Regulation (EU) 2023/1114 on Markets in Crypto-Assets (MiCA). https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32023R1114
- U.S. President's Working Group on Financial Markets. (2021). Report on Stablecoins. https://home.treasury.gov/system/files/136/StableCoinReport_Nov1_508.pdf
- Bank of England. (2023). The Regulatory Regime for Systemic Payment Systems Using Stablecoins. https://www.bankofengland.co.uk/

### CBDC Research
- Federal Reserve Bank of Boston & MIT DCI. (2022). Project Hamilton Phase 1. https://www.bostonfed.org/publications/one-time-pubs/project-hamilton-phase-1-executive-summary.aspx
- European Central Bank. (2023). A Stocktake on the Digital Euro. https://www.ecb.europa.eu/paym/digital_euro/
- Atlantic Council. (2025). Central Bank Digital Currency Tracker. https://www.atlanticcouncil.org/cbdctracker/

### Industry Reports
- Castle Island Ventures & Brevan Howard Digital. (2024). Stablecoins: The Emerging Market Story. https://castleisland.vc/stablecoins-the-emerging-market-story/
- Circle. (2025). USDC Transparency and Attestation Reports. https://www.circle.com/en/transparency
- DefiLlama. (2025). Stablecoins Dashboard. https://defillama.com/stablecoins

---

## Computational Exercises

The following notebooks provide hands-on implementations of concepts covered in this section:

- **`notebooks/05-defi-protocols.ipynb`** (upcoming) — Simulate MakerDAO vault operations, including collateral deposit, DAI minting, stability fee accrual, and liquidation mechanics. Model the relationship between collateralization ratios, liquidation thresholds, and user outcomes under various market scenarios. Implement a vault management simulator that tracks portfolio value through historical ETH price data and identifies when liquidations would trigger.

- **`notebooks/13-stablecoin-analysis.ipynb`** (upcoming) — Analyze stablecoin peg stability using historical price data. Build a peg deviation tracker across USDT, USDC, and DAI that measures maximum depeg events, time-to-recovery, and correlation with market stress. Simulate the Terra/UST death spiral mechanics using a simplified mint/burn model to demonstrate how reflexivity amplifies depegging. Implement a stablecoin reserve adequacy framework that evaluates collateral quality, liquidity, and redemption capacity. Compare fiat-collateralized, crypto-collateralized, and algorithmic stablecoin designs across risk metrics including Sharpe ratio of peg maintenance, maximum drawdown, and recovery time.
