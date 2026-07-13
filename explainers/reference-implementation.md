# Reference Implementation: UTXO6-DNS

**Status:** Active — v0.1.0 (Public Preview)

**Repository:** [`cocafoundation6/utxo6-dns`](https://github.com/cocafoundation6/utxo6-dns)

**License:** Apache License 2.0

---

## 1. Overview

This document describes the **reference implementation** of the UW2I (UTXO Web Wallet Interoperability) Community Group specifications.

The implementation is provided by the **UTXO6-DNS** open‑source project, which serves as a production‑ready codebase demonstrating:

- UTXO‑DNS resolution (`.utxo` domain → multi‑chain address mapping)
- vLEI credential verification
- Zero‑knowledge proof utilities for data authorization
- RWA (Real‑World Asset) tokenization via Solidity smart contracts
- CoCaDEX‑RWA compute power marketplace

All components are designed to align with the IETF UTXO‑DNS wire protocol (`draft-guorong-utxo6-dns-20`) and the UW2I wallet–web messaging protocol defined by this CG.

---

## 2. Implementation Mapping

The following table maps UW2I CG deliverables to their corresponding implementation artifacts in the `utxo6-dns` repository.

| UW2I CG Deliverable | Implementation Artifact | Location |
| :--- | :--- | :--- |
| **UTXO‑DNS Resolution** | `UTXOResolver` class — resolves `.utxo` domains to multi‑chain addresses (Ethereum, Solana, Bitcoin) with caching and fallback support. | [`packages/compute-market/src/utils/utxoResolver.ts`](https://github.com/cocafoundation6/utxo6-dns/blob/main/packages/compute-market/src/utils/utxoResolver.ts) |
| **vLEI Credential Verification** | `VLEIVerifier` class — integrates with GLEIF API for real‑time vLEI status checking (batch queries supported). | [`packages/compute-market/src/utils/vleiVerifier.ts`](https://github.com/cocafoundation6/utxo6-dns/blob/main/packages/compute-market/src/utils/vleiVerifier.ts) |
| **Zero‑Knowledge Proof Utilities** | `ZKProof` class — generates and verifies access keys for privacy‑preserving data authorization. | [`packages/compute-market/src/utils/zkProof.ts`](https://github.com/cocafoundation6/utxo6-dns/blob/main/packages/compute-market/src/utils/zkProof.ts) |
| **Compute RWA Tokenization** | `ComputeRWA.sol` — Solidity smart contract for tokenizing GPU/CPU compute power as divisible RWA assets. | [`packages/compute-market/src/contracts/ComputeRWA.sol`](https://github.com/cocafoundation6/utxo6-dns/blob/main/packages/compute-market/src/contracts/ComputeRWA.sol) |
| **Data Licensing & Authorization** | `DataLicense.sol` — Solidity smart contract for data UTXO licensing and authorization management. | [`packages/compute-market/src/contracts/DataLicense.sol`](https://github.com/cocafoundation6/utxo6-dns/blob/main/packages/compute-market/src/contracts/DataLicense.sol) |
| **Compute Assetization Engine** | `ComputeAssetizationEngine` — TypeScript core engine for compute power assetization, lease management, and availability tracking. | [`packages/compute-market/src/core/ComputeAssetizationEngine.ts`](https://github.com/cocafoundation6/utxo6-dns/blob/main/packages/compute-market/src/core/ComputeAssetizationEngine.ts) |
| **Data Marketplace** | `DataMarketplace` — TypeScript core engine for data UTXO creation, licensing, and search. | [`packages/compute-market/src/core/DataMarketplace.ts`](https://github.com/cocafoundation6/utxo6-dns/blob/main/packages/compute-market/src/core/DataMarketplace.ts) |
| **Filtering & Fair Value Engine** | `FilterAndPricingEngine` — TypeScript engine for data quality assessment, compliance checking, and dynamic pricing. | [`packages/compute-market/src/core/FilterAndPricingEngine.ts`](https://github.com/cocafoundation6/utxo6-dns/blob/main/packages/compute-market/src/core/FilterAndPricingEngine.ts) |
| **AI Service Aggregator** | `AIServiceAggregator` — TypeScript engine for AI service provider aggregation with cost‑quality routing. | [`packages/compute-market/src/core/AIServiceAggregator.ts`](https://github.com/cocafoundation6/utxo6-dns/blob/main/packages/compute-market/src/core/AIServiceAggregator.ts) |
| **Trading Side Core** | `ComputeTradingEngine` — TypeScript engine for compute trading, data license trading, and market data aggregation. | [`packages/compute-market/src/core/ComputeTradingEngine.ts`](https://github.com/cocafoundation6/utxo6-dns/blob/main/packages/compute-market/src/core/ComputeTradingEngine.ts) |
| **Global Performance Testnet** | End‑to‑end test environment with multi‑region deployment, load generation, and monitoring. | [`testnet/`](https://github.com/cocafoundation6/utxo6-dns/tree/main/testnet) |

---

## 3. Architecture Overview
┌─────────────────────────────────────────────────────────────────┐
│                       Web Application                           │
└─────────────────────────────┬───────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                     UW2I Wallet–Web Protocol                    │
│                (JSON‑based messaging, postMessage)              │
└─────────────────────────────┬───────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      UTXO6-DNS Resolver                         │
│ • Resolves .utxo domains to multi‑chain addresses               │
│ • Verifies VRF proofs and Merkle proofs                         │
│ • Caches results with TTL                                       │
└─────────────────────────────┬───────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                     Core Engines (TypeScript)                   │
│   ┌───────────────┐ ┌───────────────┐ ┌───────────────────┐     │
│   │   Compute     │ │     Data      │ │     Filter &      │     │
│   │ Assetization  │ │  Marketplace  │ │   Fair Value      │     │
│   └───────────────┘ └───────────────┘ └───────────────────┘     │
│   ┌───────────────┐ ┌───────────────┐ ┌───────────────────┐     │
│   │  AI Service   │ │   Trading     │ │     UTXO-DNS      │     │
│   │  Aggregator   │ │    Engine     │ │     Utilities     │     │
│   └───────────────┘ └───────────────┘ └───────────────────┘     │
└─────────────────────────────┬───────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                  Smart Contracts (Solidity)                     │
│        ┌─────────────────┐ ┌─────────────────┐                  │
│        │ ComputeRWA.sol  │ │ DataLicense.sol │                  │
│        └─────────────────┘ └─────────────────┘                  │
└─────────────────────────────────────────────────────────────────┘

---

## 4. Installation & Usage

### Prerequisites

- Node.js v18+
- npm or yarn

### Installation

```bash
git clone https://github.com/cocafoundation6/utxo6-dns.git
cd utxo6-dns
npm install

Build

npm run build

Run Tests

npm test

Start Testnet

./testnet/scripts/deploy-testnet.sh

5. Integration Points
5.1 Wallet–Web Messaging
The reference implementation includes all components required to support the UW2I messaging protocol:

Message Type	Implementation	Location
resolve	UTXOResolver.resolve()	packages/compute-market/src/utils/utxoResolver.ts
authenticate	Uses vLEI verification + UTXO address signature	packages/compute-market/src/utils/vleiVerifier.ts
pay	Uses ComputeTradingEngine for compute payments, DataMarketplace for data licensing	packages/compute-market/src/core/ComputeTradingEngine.ts
credential	VLEIVerifier + ZKProof for selective disclosure	packages/compute-market/src/utils/vleiVerifier.ts, packages/compute-market/src/utils/zkProof.ts
5.2 Multi‑Chain & Multi‑Token Support
The resolver supports:

Bitcoin — address format: btc:bc1...

Ethereum — address format: eth:0x...

Solana — address format: sol:...

Stablecoins — token‑specific address resolution via token field

5.3 vLEI Credential Integration
The verifier supports:

Real‑time status checking via GLEIF API

Batch verification

Selective disclosure using ZK proofs

Caching with TTL ≤ 300 seconds (configurable)

6. Relationship to W3C UW2I CG
The utxo6-dns reference implementation serves as:

Validation: Demonstrates that the UW2I CG specifications are implementable in real‑world production environments.

Feedback: Provides implementation‑driven feedback to refine the specifications.

Tooling: Offers a ready‑to‑use codebase that wallet and web application developers can adopt or extend.

Testbed: The included Global Performance Testnet serves as an end‑to‑end testing ground for interoperability.

The implementation is maintained by the CoCa Foundation in collaboration with the UW2I CG participants.

7. Contributing
Contributions to the reference implementation are welcome via:

Issues: Bug reports and feature requests

Pull Requests: Code contributions

Please see the CONTRIBUTING.md for guidelines.

8. License
The utxo6-dns reference implementation is licensed under Apache License 2.0.

9. Contacts
Role	Contact
Project Lead	Guorong Tian — foundation@utxocp.cn
W3C CG Liaison	Monica — standards@utxocd.com
GitHub Issues	cocafoundation6/utxo6-dns/issues
Related Resources:

UTXO6-DNS Repository

UW2I Community Group Charter

IETF Draft: draft-guorong-utxo6-dns-20

Global Performance Testnet

text

---

docs: add reference implementation explainer linking to utxo6-dns



