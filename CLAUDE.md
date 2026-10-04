# Cryptocurrency Education Project

## Purpose

Replicate MIT Sloan's "Blockchain and Crypto Applications: From Decentralized Finance to Web 3" course using entirely open-source resources. All content must be rigorously sourced and documented.

Course reference: MIT Sloan Blockchain and Crypto Applications Online Short Course

## Environment

- **Conda env:** `crypto-course` (activate with `mamba activate crypto-course`)
- **Prefer mamba** over conda for all package/environment operations
- **Python:** 3.11
- **Environment file:** `environment.yml` at project root
- **Notebooks run in:** JupyterLab
- **Claude Skills** Make recommendations for project and general skills to add as identified, especially those that would significantly impact token usage.

## Project Structure

```
Cryptocurrency/
├── CLAUDE.md              # This file - project config, status, standards
├── README.md              # Course overview and learning paths
├── environment.yml        # Single project-wide mamba/conda environment
├── notebooks/             # Jupyter notebooks (sequential, flat)
│   ├── 01-cryptographic-primitives.ipynb
│   ├── 02-bitcoin-blockchain-analysis.ipynb
│   └── ...
└── sections/              # Markdown content sections (sequential, flat)
    ├── 01-historical-evolution.md
    ├── 02-bitcoin-deep-dive.md
    └── ...
```

## Content Standards

### Markdown Sections
- All technical terms defined in markdown callouts (`> **Definition: Term**`)
- All acronyms expanded on first use: Full Name (ACRONYM). This matches the rule in the AI training program (its D24) and also covers Claude's replies in sessions on this repo, each reply counting as one document. Headings, titles of works, names not used as abbreviations, file names, and units are exempt.
- Comprehensive source citations: **Source:** Author. (Year). Title. URL
- Clear progression from foundational concepts to advanced details
- Cross-references to relevant notebooks
- End with Key Takeaways, Further Reading, and Computational Exercises

### Jupyter Notebooks
- Title cell with overview, learning objectives, prerequisites, estimated time
- Theory explanation in markdown cells before code
- Clear comments in code cells
- Executable examples with output demonstrations
- Exercises section with starter code
- Summary and next steps at the end

### Pedagogical Approach
- Build from basic definitions to advanced technical details
- Use concrete examples and step-by-step math before abstraction
- Connect historical context to modern implementations
- Use real-world data and statistics where possible
- Explain common conceptual hurdles explicitly (e.g., "computationally infeasible" vs "impossible")

## Progress

### Sections (markdown content)
| # | File | Status |
|---|------|--------|
| 01 | historical-evolution.md | Complete |
| 02 | bitcoin-deep-dive.md | Complete |
| 03 | ethereum-smart-contracts.md | Complete |
| 04 | blockchain-economics.md | Complete |
| 05 | platform-comparison.md | Complete |
| 06 | privacy-technologies.md | Complete |
| 07 | stablecoins.md | Complete |
| 08 | dao-governance.md | Complete |
| 09 | sustainability.md | Complete |

### Notebooks (computational)
| # | File | Status |
|---|------|--------|
| 01 | cryptographic-primitives.ipynb | Complete |
| 02 | bitcoin-blockchain-analysis.ipynb | Complete |
| 03 | ethereum-evm-analysis.ipynb | Complete |
| 04 | smart-contract-development.ipynb | Complete |
| 05 | defi-protocols.ipynb | Complete |
| 06 | market-analysis.ipynb | Complete |
| 07 | mining-economics.ipynb | Complete |
| 08 | valuation-models.ipynb | Complete |
| 09 | privacy-forensics.ipynb | Complete |
| 10 | cryptoeconomic-modeling.ipynb | Complete |
| 11 | tokenomics.ipynb | Complete |
| 12 | governance-simulation.ipynb | Complete |
| 13 | stablecoin-analysis.ipynb | Complete |
| 14 | consensus-simulations.ipynb | Complete |
| 15 | multichain-analysis.ipynb | Complete |
| 16 | energy-sustainability.ipynb | Complete |

## Notes outside the repo

The Obsidian hub note for this project is `~/Notes/1-Projects/crypto-course/crypto-course.md` (the vault is on Google Drive; `~/Notes` is a symlink). This repo is the project's home and the hub note is a pointer to it ("How I Work"). Everything about the project lives here: decisions, progress, working notes, paper notes, and research output. Do not copy repo content into the vault. The vault keeps only what cannot sensibly live in git, chiefly meeting notes and anything about people; ask before writing anything else there.

- At session start, read the hub note and its latest meeting note; carry any new decision into this repo's records.
- When a session changes the state of the project, append one line to the hub's Status log, `- YYYY-MM-DD: sentence; next: sentence.`, citing this repo's decision numbers rather than restating them, and bump `updated`.
- Keep the hub's "Where things live" links current when key files are added or renamed.
