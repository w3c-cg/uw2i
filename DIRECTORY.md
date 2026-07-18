
```markdown
# Repository Directory Structure & File Index Guide

**Version:** 1.1  
**Applicable to:** `w3c-cg/uw2i` (W3C Spec Repo) & `cocafoundation6/utxo6-dns` (Implementation Repo)  
**Purpose:** Standardize directory layouts, define file path responsibilities, and provide a queryable index for historically uploaded projects.

---

## 1. Design Objectives

- **Unified Standards:** Clear division of responsibilities between the specification repository and the implementation repository.
- **Scalability:** Ability to add new modules without disrupting existing paths.
- **Traceability:** Every file path can be quickly located, supporting version tracking and historical queries.

---

## 2. Repository 1: `w3c-cg/uw2i` (W3C Specification Repository)

*This repository stores **specification documents**, **WebIDL definitions**, and **explainers** only. It contains no operational implementation code.*

### 2.1 Recommended Directory Layout

```
uw2i/
├── README.md                     # Project overview, status, and links
├── CONTRIBUTING.md               # Contributor guidelines (references W3C CLA)
├── LICENSE                       # W3C Document License (typically CC-BY)
├── spec/                         # Specification source files
│   ├── index.bs                  # Main specification (Bikeshed format)
│   ├── compute-market.bs         # CoCaDEX compute market spec (new)
│   ├── data-utxo.bs              # Data UTXO spec (if added)
│   └── images/                   # Diagrams used in the spec
│       ├── architecture.svg
│       └── flow.svg
├── explainers/                   # Non-normative explanatory documents
│   ├── charter.md                # Community Group charter
│   ├── use-cases.md              # Overall use cases
│   ├── compute-market-explainer.md  # Compute market use cases
│   └── diagrams/                 # Supplementary diagrams
│       ├── compute-assetization.puml
│       └── data-license-flow.png
├── api/                          # WebIDL definitions
│   ├── utxo.idl                  # Base UTXO interfaces
│   ├── compute-market.idl        # Compute market interfaces
│   └── data-utxo.idl             # Data UTXO interfaces
└── tests/                        # (Optional) Specification validation tests (e.g., WPT)
    └── idl-validity/             # WebIDL syntax validation
```

### 2.2 Sorted File Path List (Alphabetical Order)

| Path | Description |
|------|-------------|
| `api/compute-market.idl` | CoCaDEX compute assetization, leasing, and trading interfaces |
| `api/data-utxo.idl` | Data UTXO authorization and licensing interfaces |
| `api/utxo.idl` | Public UTXO base types |
| `explainers/charter.md` | Community Group charter |
| `explainers/compute-market-explainer.md` | Compute market usage scenarios |
| `explainers/diagrams/compute-assetization.puml` | Assetization flowchart (PlantUML) |
| `explainers/diagrams/data-license-flow.png` | Data license flow (PNG) |
| `explainers/use-cases.md` | Aggregated use cases |
| `spec/compute-market.bs` | Compute market specification draft (Bikeshed) |
| `spec/data-utxo.bs` | Data UTXO specification draft |
| `spec/images/architecture.svg` | System architecture diagram |
| `spec/images/flow.svg` | Transaction flow diagram |
| `spec/index.bs` | Main specification entry point (references others) |
| `README.md` | Repository home page |
| `CONTRIBUTING.md` | Contribution guide (links to W3C) |
| `LICENSE` | License file |

---

## 3. Repository 2: `cocafoundation6/utxo6-dns` (Implementation Repository)

*This repository stores **protocol implementation code**, **smart contracts**, **tests**, and **examples**.*

### 3.1 Recommended Directory Layout

```
utxo6-dns/
├── README.md
├── CONTRIBUTING.md
├── LICENSE                      # Apache-2.0
├── packages/                    # Monorepo structure
│   ├── core/                    # UTXO6-DNS core resolution library
│   │   ├── src/
│   │   │   ├── resolver.ts
│   │   │   ├── vrf.ts
│   │   │   └── types.ts
│   │   ├── tests/
│   │   ├── package.json
│   │   └── tsconfig.json
│   ├── compute-market/          # CoCaDEX Compute Configuration Center (Primary Upload)
│   │   ├── src/
│   │   │   ├── core/
│   │   │   │   ├── ComputeAssetizationEngine.ts
│   │   │   │   ├── DataMarketplace.ts
│   │   │   │   ├── FilterAndPricingEngine.ts
│   │   │   │   ├── AIServiceAggregator.ts
│   │   │   │   └── ComputeTradingEngine.ts
│   │   │   ├── contracts/
│   │   │   │   ├── ComputeRWA.sol
│   │   │   │   └── DataLicense.sol
│   │   │   ├── utils/
│   │   │   │   ├── utxoResolver.ts
│   │   │   │   ├── vleiVerifier.ts
│   │   │   │   └── zkProof.ts
│   │   │   └── index.ts
│   │   ├── tests/
│   │   │   ├── unit/
│   │   │   └── integration/
│   │   ├── package.json
│   │   └── README.md
│   └── sdk/                     # (Optional) External SDK wrapper
│       ├── src/
│       └── package.json
├── docs/                        # Protocol documentation (non-code)
│   ├── api/                     # Auto-generated API docs
│   └── protocol/                # Protocol design specifications
├── examples/                    # Usage examples
│   ├── basic-usage.ts
│   └── compute-lease-example.ts
└── scripts/                     # Build/helper scripts
    ├── generate-index.sh
    └── deploy-contracts.js
```

### 3.2 Sorted File Path List (By Module)

#### Core Resolution Library (`core`)
- `packages/core/src/resolver.ts` – UTXO-DNS resolver
- `packages/core/src/vrf.ts` – VRF function implementation
- `packages/core/src/types.ts` – Public type definitions
- `packages/core/tests/` – Unit tests
- `packages/core/package.json` – Dependency configuration

#### Compute Market Module (`compute-market`)
- `packages/compute-market/src/core/ComputeAssetizationEngine.ts` – Compute assetization engine
- `packages/compute-market/src/core/DataMarketplace.ts` – Data license trading marketplace
- `packages/compute-market/src/core/FilterAndPricingEngine.ts` – Fair value discovery & pricing
- `packages/compute-market/src/core/AIServiceAggregator.ts` – Multi-AI service aggregation routing
- `packages/compute-market/src/core/ComputeTradingEngine.ts` – Trade matching engine
- `packages/compute-market/src/contracts/ComputeRWA.sol` – Compute RWA smart contract (Solidity)
- `packages/compute-market/src/contracts/DataLicense.sol` – Data license contract
- `packages/compute-market/src/utils/utxoResolver.ts` – UTXO resolution utilities
- `packages/compute-market/src/utils/vleiVerifier.ts` – VLEI credential verification
- `packages/compute-market/src/utils/zkProof.ts` – Zero-knowledge proof helpers
- `packages/compute-market/src/index.ts` – Module entry point
- `packages/compute-market/tests/unit/` – Unit tests
- `packages/compute-market/tests/integration/` – Integration tests
- `packages/compute-market/package.json`
- `packages/compute-market/README.md`

#### Others
- `packages/sdk/` – SDK (if developed later)
- `docs/api/` – TypeDoc generated API references
- `docs/protocol/` – Protocol design documents
- `examples/basic-usage.ts` – Basic invocation example
- `examples/compute-lease-example.ts` – Compute leasing example
- `scripts/generate-index.sh` – Shell script to generate file index
- `scripts/deploy-contracts.js` – Contract deployment script

---

## 4. Establishing a "File Path Index" for Easy Queries

It is recommended to maintain an **`INDEX.md`** or **`DIRECTORY.md`** file in the root of each repository, listing all file paths with brief descriptions. This file can be automatically generated via script.

### 4.1 Auto-Generation Script (Example)

Place the following script in `utxo6-dns/scripts/generate-index.sh`:

```bash
#!/bin/bash
# Generate repository file index (excluding node_modules, dist, etc.)
echo "# File Index" > INDEX.md
echo "Generated on $(date)" >> INDEX.md
echo "" >> INDEX.md
echo "| Path | Description |" >> INDEX.md
echo "|------|-------------|" >> INDEX.md

find . -type f \
  -not -path "./node_modules/*" \
  -not -path "./dist/*" \
  -not -path "./.git/*" \
  -not -path "./coverage/*" \
  -not -name "INDEX.md" \
  -not -name "package-lock.json" \
  | sort \
  | while read -r file; do
    # Extract filename as a brief description (can be manually supplemented)
    desc=$(basename "$file" | sed 's/\.[^.]*$//' | tr '_-' ' ')
    echo "| \`$file\` | $desc |" >> INDEX.md
done
```

Run it to generate a Markdown table that can be previewed directly on GitHub.

---

## 5. Query Methods

| Scenario | Method |
|------|-------------|
| Search by filename | Use the GitHub search bar (top of page) and enter the filename (e.g., `ComputeAssetizationEngine.ts`) |
| Browse by path | Navigate the directory tree directly, referencing the structure diagram in `README.md` |
| Search by content keyword | Use `git grep` or GitHub's "Code" search functionality |
| View historical versions | Use `git log -- <file>` or GitHub's "Blame" view |
| Offline file list | Run `find . -type f | sort` and export to a file |

---

## 6. Maintenance Recommendations

1.  **After adding new files**, promptly update the `README.md` of the corresponding submodule and re-run the `generate-index.sh` script to refresh the index.
2.  **File naming conventions**: Use lowercase letters and hyphens for specification files (e.g., `compute-market.bs`), or follow language-specific conventions (e.g., CamelCase for TypeScript files).
3.  **Before each commit**, check whether any new files are missing from the index.

---

By following this directory design and indexing mechanism, you can efficiently manage historically uploaded projects across both repositories, enabling rapid path queries and precise version traceability. If you need to adjust specific files or add new modules, feel free to reach out.
```

---

