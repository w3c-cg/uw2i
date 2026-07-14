# Linked Identity Compliance Verification Framework

**Authors:** Tian Guorong 
**Supporting Organization:** Hong Kong Ronghua International Group Co., Ltd.  
**Related IETF Draft:** draft-guorong-utxo-dns-01  
**Reference Implementation:** [UTXO6-DNS v0.1.0-alpha](https://github.com/cocafoundation6/utxo6-dns/releases/tag/v0.1.0-alpha)

---

## Overview

The **Linked Identity Compliance Verification** framework is a lightweight, economically efficient compliance verification model for HKDAP stablecoin cross-border payments. It is fully aligned with the Hong Kong Monetary Authority's (HKMA) stablecoin issuer sandbox requirements and serves as a reference implementation for the UTXO6-DNS protocol (IETF draft-guorong-utxo-dns-01).

This framework transforms the traditional "per-transaction KYC" model into a **chain-based identity verification** model, where each participant registers their `.utxo` domain and vLEI credential once, and the entire payment chain is validated at the transaction level.

---

## Key Concepts

### 1. Linked Identity Chain

Every participant in a payment transaction (issuer, sender, receiver, gateway) is bound via:

- **`.utxo` Domain** — DNS-based identity anchor
- **vLEI Credential** — GLEIF-issued Legal Entity Identifier (verifiable credential)
- **VRF IID** — Verifiable Random Function-based IPv6 Interface Identifier (IETF §4)

┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│    Issuer   │───▶│   Sender    │───▶│    Channel  │───▶ │   Receiver  │
│    (HKDAP)  │     │  (.utxo)    │     │  (IP-to-IP) │     │   (.utxo)   │
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
       │                  │                     │                  │
       ▼                  ▼                     ▼                  ▼
┌─────────────────────────────────────────────────────────────────────────┐
│            vLEI + .utxo Domain + VRF IID (IETF §4 + §10)                │
│                                                                         │
│ ▶ Validation: Any link failure → Entire payment chain rejected         │
└────────────────────────────────────────────────────────────——─——────────┘

### 2. One-Time Registration, Reusable Verification

- Each participant registers identity **once**
- Registration includes: `.utxo` domain resolution, vLEI credential verification, VRF IID generation
- Registered identity is **reused across all subsequent transactions**
- Reduces per-transaction compliance overhead from 350-650ms to **<100ms**

### 3. Chain-Based Validation

- The entire identity chain (issuer → sender → receiver) is validated per transaction
- Any failure in any link → entire payment is rejected
- Provides end-to-end compliance assurance

---

## HKMA Sandbox Alignment

| HKMA Requirement | Implementation | Status |
|------------------|----------------|--------|
| Identity verified by licensed entity/regulated FI/reliable third party | `.utxo` domain resolution + vLEI verification via GLEIF API | ✅ |
| Closed-loop arrangement for initial testing | Pre-registered identity chain; only authorized participants can transact | ✅ |
| Real-world use case validation | Cross-border HKDAP trade settlement and EV charging micro-payments | ✅ |
| Reserve asset real-time auditability | PRNAUDIT RR daily audit summary with Merkle root | ✅ |
| AML/CFT compliance | TravelRule reporting (≥ HKD 8,000) + sanctions screening | ✅ |
| User protection and redemption | Smart contract automatic redemption mechanism | ✅ |

---

## Architecture

### Module Structure
linked-identity/
├── verifier.py # Core linked identity verifier
├── models.py # Data models (IdentityRecord, ChainResult, etc.)
└── utils.py # VRF IID generation, Merkle root computation

### Payment Integration
hkdap/
└── payment.py # HKDAP payment with linked identity validation

### Regulatory Reporting
prn/
└── audit.py # PRNAUDIT daily summary (IETF §7)

### Compliance
compliance/
└── travel_rule.py # FATF IVMS101 integration

---

## Reference Implementation

The complete reference implementation is available in the CoCa Foundation repository:

| Component | Location |
|-----------|----------|
| **Repository** | `https://github.com/cocafoundation6/utxo6-dns` |
| **Release** | `v0.1.0-alpha` |
| **Core Module** | `linked-identity/` |
| **Payment Module** | `hkdap/` |
| **Audit Module** | `prn/` |

### Quick Start

```python
from linked_identity.verifier import LinkedIdentityVerifier, IdentityRole
from hkdap.payment import HKDAPLinkedPayment, PaymentPurpose

# Initialize verifier
verifier = LinkedIdentityVerifier()

# Register identities (one-time)
verifier.register_identity("issuer.utxo", "vlei_jwt_1", IdentityRole.ISSUER)
verifier.register_identity("sender.utxo", "vlei_jwt_2", IdentityRole.SENDER)
verifier.register_identity("receiver.utxo", "vlei_jwt_3", IdentityRole.RECEIVER)

# Initialize payment processor
payment = HKDAPLinkedPayment(verifier)

# Initiate payment with linked identity verification
result = payment.initiate_payment(
    issuer_domain="issuer.utxo",
    sender_domain="sender.utxo",
    receiver_domain="receiver.utxo",
    amount_hkd=10000.0,
    purpose=PaymentPurpose.CROSS_BORDER_TRADE
)

Implementation Status
Feature	Status	Notes
.utxo domain resolution	✅ Implemented	UTXO RR (code 260)
vLEI credential verification	✅ Implemented	GLEIF API integration
VRF IID generation (IETF §4)	✅ Implemented	ECVRF-compatible
Linked identity chain validation	✅ Implemented	Chain-based validation
HKDAP payment integration	✅ Implemented	Standard Chartered Bank HKDAP
TravelRule reporting (≥ HKD 8,000)	✅ Implemented	IVMS101 format
PRNAUDIT daily summary (IETF §7)	✅ Implemented	Merkle root + 2-of-3 MPC
IP-to-IP encrypted channel	✅ Implemented	WireGuard
Docker sandbox environment	🚧 Planned	-
Regulatory dashboard (PRN-API)	🚧 Planned	-

References
IETF draft-guorong-utxo-dns-01 — UTXO Domain Name System (UTXO6-DNS)

CoCa Reference Implementation v0.1.0-alpha

HKMA Stablecoin Issuer Sandbox Arrangement

GLEIF vLEI Ecosystem Governance Framework

FATF Travel Rule — IVMS101 Standard

Contributing
Contributions are welcome! Please refer to the CoCa Foundation CONTRIBUTING.md for guidelines.

Document Version: 1.0.0
Last Updated: 2026-07-15

docs: add linked identity compliance verification explainer

Added explainers/linked-identity-compliance.md

Documents the Linked Identity Compliance Verification framework

Aligns with UTXO6-DNS v0.1.0-alpha release

References HKMA sandbox requirements and IETF draft-guorong-utxo-dns-01

Authors: Tian Guorong
Supporting Organization: Hong Kong Ronghua International Group Co., Ltd.


