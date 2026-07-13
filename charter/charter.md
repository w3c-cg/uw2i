# W3C Community Group Charter

## UTXO Web Wallet Interoperability (UW2I) Community Group

| **Charter Version** | **Date** | **Proposed by** | **Contact** |
| :--- | :--- | :--- | :--- |
| 1.0 | 3 June 2026 | Guorong Tian (CoCa Foundation) | [foundation@utxocp.cn](mailto:foundation@utxocp.cn) |

---

## 1. Group Name

> **UTXO Web Wallet Interoperability Community Group**  
> *(short name: **UW2I CG**)*

---

## 2. Brief Summary

The UTXO Web Wallet Interoperability Community Group defines open specifications for wallets (browser extensions, desktop, and mobile apps) to integrate the IETF UTXO‑DNS protocol. It enables self‑sovereign login, payment addressing, and verifiable credential presentation using `.utxo` domains, with native support for **multi‑chain wallet address interoperability** across networks such as Bitcoin, Ethereum, and Solana, as well as stablecoin payments including USDC, USDT, and Hong Kong regulated stablecoins (e.g., HKDA).

The group delivers a wallet–web messaging protocol, resolution and authentication patterns, vLEI integration best practices, and a reference implementation — all without requiring any native browser API.

---

## 3. Statement of Purpose

This Community Group (CG) enables **wallets** (browser extensions, desktop applications, and mobile wallets) to interoperate with web applications using the **UTXO‑DNS protocol** (standardised at the IETF). The CG defines open, vendor‑neutral specifications that allow wallets to:

- Resolve `.utxo` domain names to **multi‑chain blockchain endpoint identifiers**, supporting networks such as **Bitcoin (BTC), Ethereum (ETH), Solana (SOL)**, and any other blockchain that uses public key hashes or account‑based addresses.

- Perform **self‑sovereign authentication** (sign a challenge using a private key bound to a `.utxo` domain, without any third‑party identity provider).

- Construct and sign **payments** addressed to a `.utxo` domain on any supported chain, **including stablecoins such as USDC, USDT, and Hong Kong regulated stablecoins** (resolve → build chain‑appropriate transaction → request user confirmation → sign).

- Present **verifiable credentials** such as GLEIF vLEI (real‑time status, selective disclosure).

The CG does **not** specify a native browser API (e.g., `navigator.utxo`) in its initial phase. Instead, it defines a wallet–web messaging protocol based on existing Web technologies (`window.postMessage`, extension message passing). Native browser APIs may be considered in a future phase once wallet adoption is widespread.

**Multi‑chain interoperability is a first‑class requirement:** A single `.utxo` domain can be associated with different addresses on different blockchains; the wallet resolves the correct address based on the chain and **token type (e.g., USDC on Ethereum, USDT on Solana, Hong Kong stablecoin on a compliant chain)** requested by the web application.

---

## 4. Scope of Work

### In Scope

- **Wallet–Web Messaging Protocol**  
  A standardised JSON‑based message format (request/response) that web pages and wallets can exchange to request:

  - Domain resolution (specifying a target blockchain **and optionally a token identifier**, e.g., `"chain": "ethereum", "token": "USDC"` or `"chain": "solana", "token": "USDT"`).
  - Authentication (challenge signing using the private key of a `.utxo` domain).
  - Payment (to a `.utxo` domain, with amount, **token type (stablecoin or native coin)**, optional memo, and target chain).
  - Credential presentation (vLEI status, selective disclosure).

- **UTXO‑DNS Resolution for Wallets**  
  Specification of how wallets:
  - Call a UTXO‑DNS resolver (configurable, with fallback).
  - Parse UTXO resource records that contain **multi‑chain address mappings** (e.g., `btc:bc1...`, `eth:0x...`, `sol:...`).
  - Verify Merkle proofs and VRF proofs (according to `draft-guorong-utxo6-dns-20`).
  - Cache resolution results with appropriate TTL.

- **Multi‑Chain & Multi‑Token Address Interoperability**
  - How a `.utxo` domain's UTXO record encodes addresses for multiple blockchains and, **where applicable, token‑specific address information (e.g., a stablecoin wallet address on a compliant chain)**.
  - How a wallet selects the appropriate address when a web application requests payment on a specific chain **and token type**.
  - **Support for Hong Kong regulated stablecoins:** The specification will not mandate any particular stablecoin but will provide a generic `token` field that can reference any token contract address or symbol, allowing wallets and applications to support **HKDA, USDC, USDT, or future HKD‑pegged stablecoins** as they become available.
  - Error handling when a domain does not support a requested chain or token.

- **Self‑Sovereign Login Pattern**  
  How a web page can request a wallet to sign a challenge using the private key of a `.utxo` domain (independent of any specific blockchain or token), and how the web page verifies the signature (no third‑party IdP).

- **Payment Addressing Pattern (including Stablecoins)**  
  How a web page can request a wallet to pay to a `.utxo` domain, including:
  - Target chain (e.g., `"chain": "ethereum"`) and **token type** (e.g., `"token": "USDC"`, `"token": "USDT"`, or `"token": "HKDA"` for a Hong Kong dollar stablecoin), amount, optional memo.
  - How the wallet resolves the domain, obtains the correct address for that chain (and, if needed, the token contract address), constructs the transaction, prompts the user, signs, and returns a transaction hash.
  - **Stablecoin‑specific considerations:** Fee estimation (gas on Ethereum, rent on Solana), minimum balances, and compliance checks (e.g., sanction screening) are wallet responsibilities and not defined by this CG, but the messaging protocol will carry enough information for wallets to perform these checks.

- **vLEI Credential Integration**  
  How wallets fetch and verify vLEI real‑time status from GLEIF API, support selective disclosure, and optionally cache status (TTL ≤ 300 seconds). This is independent of the underlying blockchain or token type.

- **Security, Privacy and UX Guidelines**  
  Recommendations for key storage (HSM/TEE), permission prompts, user consent, data minimisation, differential privacy — applicable across multiple chains and stablecoins.

- **Reference Implementation**  
  Open‑source browser extension (Chrome/Edge) and a demo web page, supporting at least three chains (e.g., BTC, Ethereum, Solana) **and at least two stablecoins (e.g., USDC on Ethereum, USDT on Solana)** to demonstrate multi‑chain and multi‑token interoperability. Licensed under Apache‑2.0 or MIT.

- **Interoperability Test Suite**  
  A set of test cases and a tool for wallet developers to verify compliance with resolution, multi‑chain addressing, **and stablecoin payment requests**.

### Out of Scope (Initial Phase)

- Native browser APIs (e.g., `navigator.utxo`). These may be addressed in a future charter update once the wallet ecosystem has matured.
- The UTXO‑DNS wire protocol itself (delegated to IETF DNSOP/6man working groups).
- Blockchain consensus algorithms, tokenomics, or any commercial incentive model.
- Modifications to DNS message formats or transport protocols.
- Chain‑specific transaction format details beyond address resolution (wallets are expected to implement their own chain logic).
- **Regulatory compliance beyond providing a framework** (e.g., sanction screening, KYC/AML rules are wallet implementer responsibilities).

---

## 5. Deliverables

| Deliverable | Type | Timeline (from CG formation) |
| :--- | :--- | :--- |
| Wallet–Web Messaging Protocol (v1.0) | CG Draft Report | 3 months |
| UTXO‑DNS Wallet Resolution Specification (incl. multi‑chain address mapping) | CG Draft Report | 3 months |
| Multi‑Chain & Multi‑Token Address Interoperability Note (incl. stablecoins) | Informative Note | 4 months |
| Self‑Sovereign Login with UTXO‑DNS | Informative Note | 4 months |
| Payment Addressing via UTXO‑DNS (multi‑chain + stablecoins) | Informative Note | 4 months |
| vLEI Credential Integration Best Practices | Informative Note | 5 months |
| Reference Implementation (browser extension + demo, BTC/ETH/SOL + USDC/USDT) | Open‑Source Code | 5 months |
| Interoperability Test Suite | Tool | 6 months |

> All deliverables are subject to rough consensus of the CG and may be adjusted based on community feedback.

---

## 6. Dependencies and Liaisons

| Organisation / Group | Relationship | Work to coordinate |
| :--- | :--- | :--- |
| **IETF DNSOP WG** | Upstream | Follow `draft-guorong-utxo6-dns-20`; ensure wallet specifications align with final RR type and EDNS0 option. |
| **IETF 6man WG** | Upstream | Follow VRF‑based IID generation (IPv6 address anchoring). |
| **GLEIF** | External identity source | Coordinate on vLEI real‑time status API (batch queries, webhooks, performance requirements). |
| **W3C Web Payments WG** | Future liaison | Once wallet interoperability is stable, discuss how Payment Request API could leverage the UW2I messaging protocol. |
| **OpenWallet Foundation** | Implementation | Share open‑source components; participate in interoperability testing. |
| **Chain‑specific communities (Bitcoin, Ethereum, Solana, etc.)** | Ecosystem outreach | Ensure address format compatibility and gather real‑world requirements. |
| **Stablecoin issuers (Circle, Tether, HKDA issuer, etc.)** | Ecosystem outreach | Coordinate on token contract address standards, fee estimation, and compliance hooks (optional). |

---

## 7. Communication and Decision Process

- **Public Mailing List:** `public-utxo-wallet@w3.org` (to be created). All technical discussions and decisions are archived here.

- **Teleconferences:** Bi‑weekly (60 minutes), with public minutes and optional recording. Dial‑in details will be posted on the mailing list at least 48 hours in advance.

### Decision Making

- **Rough Consensus** is the primary method. A proposal is considered adopted if no sustained objection is raised after a reasonable discussion period (minimum 7 days for substantive issues).

- **Formal votes** may be used only as a last resort when rough consensus cannot be determined. Votes require a quorum of 50% of active participants and a simple majority.

### Chair

A chair will be elected by participants within the first month of CG operation. The chair is responsible for agenda setting, facilitating consensus, and liaising with W3C staff.

---

## 8. Participation

- **Eligibility:** Any individual or organisation interested in wallets, multi‑chain interoperability, **stablecoin payments**, decentralised identity, Web payments, or related standards may join. No fee is required.

- **How to join:** Subscribe to the mailing list and attend at least one teleconference. Active participants are those who have contributed to mailing list discussions, attended meetings, or submitted pull requests to the CG's GitHub repository.

### Roles

- **Participant:** Any subscriber.
- **Contributor:** Participant who has provided a deliverable (spec text, code, test, use case) that is adopted by the CG.
- **Editor:** Appointed by the CG to maintain a specific deliverable.
- **Chair:** Elected as described above.

---

## 9. Intellectual Property and Licensing

- **Specifications** produced by the CG are subject to the **W3C Community Contributor License Agreement (CLA)**. All participants agree to license their contributions under the CLA.

- **Reference implementation** code will be licensed under **Apache‑2.0 or MIT** (explicitly stated in the repository).

- **Test suite** will be licensed under the **W3C Software and Document Notice** or an open‑source license determined by the CG.

---

## 10. Termination and Renewal

- This charter is effective upon approval by the W3C Community Development Lead.

- The CG may be terminated by a two‑thirds majority vote of active participants or by W3C staff if the group becomes inactive (no meetings or mailing list traffic for six consecutive months).

- The charter may be renewed or amended by the same rough consensus process used for technical decisions.

---

## 11. Community Supporters

An initial list of supporting participants:

1. **Mr. Zhibin Lei**, Secretary-General of Institute of Web3.0 Hong Kong — [zblei@ust.hk](mailto:zblei@ust.hk)

2. **Ms. Yidan Zhu**, Hong Kong Basic Currency Limited — [monica0615@qq.com](mailto:monica0615@qq.com)

3. **Mr. Xinfeng Huang**, Hong Kong Ronghua International Group Co., Ltd. — [1421798706@qq.com](mailto:1421798706@qq.com)

4. **Mr. AHMAD NAUMAN**, EXUKTSOFT CONSULTANTS CO., LTD (Vietnam) — [nomy_790@yhoo.com](mailto:nomy_790@yhoo.com)

5. **Mr. Sheng Guo**, Zhongshiyuan (Hainan Special Economic Zone) Tourism Group Co., Ltd. — [guo11907360@qq.com](mailto:guo11907360@qq.com)

6. **Ms. POM SPOM SAMPHORS**, PT COCA PETROLEUM RESOURCES (Indonesia) — [cocapetroleum@gmail.com](mailto:cocapetroleum@gmail.com)

7. **Mr. Guorong Tian, Mr. Fanyin Yang, Mr. Ye Yang, Mr. Xian Li, and Ms. YASMEEN MOHAMMED A ALMARRI**, CoCa Global Joint Foundation

---

## feat: add UW2I Community Group Charter v1.0

**Contact for this proposal:**  
Monica — [standards@utxocd.com](mailto:standards@utxocd.com)  
cc: [foundation@utxocp.cn](mailto:foundation@utxocp.cn)
