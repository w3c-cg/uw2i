<img width="640" height="640" alt="ecad56f1e8304231ee99568b916d30f3" src="https://github.com/user-attachments/assets/edb6c5aa-18c2-4071-a105-973352604229" />


<h1 align="center">UTXO Web Wallet Interoperability Community Group (UW2ICG)</h1>

<p align="center">
  <strong>🌐 Bridging <span style="color:#0056b3;">Crypto</span> and <span style="color:#0056b3;">Regulated Fiat Wallets</span></strong>
</p>

This repository hosts the work of the **UTXO Web Wallet Interoperability Community Group** (UW2ICG), a W3C Community Group focused on defining **<span style="color:#0056b3;">open, vendor‑neutral specifications</span>** for **<span style="color:#0056b3;">cryptocurrency and fiat‑compliant wallets</span>** to integrate the IETF UTXO‑DNS protocol.

The group's deliverables enable:

- **<span style="color:#0056b3;">Compliant Digital Asset Payments</span>** – Seamless payments via **Web Payments API extension (`utxo-payment`)**, supporting both **native cryptocurrencies** and **<span style="color:#0056b3;">regulated stablecoins</span>** (**<span style="color:#0056b3;">USDC, USDT, HKDA</span>**).

- **<span style="color:#0056b3;">Self‑Sovereign Identity with KYC/AML</span>** – **Verifiable Credentials (vLEI)** integrated with **<span style="color:#0056b3;">real‑time compliance checks</span>**, enabling secure, regulated identity for both individuals and legal entities.

- **<span style="color:#0056b3;">Cross‑Border Payment Routing</span>** – **Jurisdiction‑aware address resolution** with built‑in **<span style="color:#0056b3;">compliance hooks</span>** (**<span style="color:#0056b3;">sanction screening</span>**, **<span style="color:#0056b3;">transaction monitoring</span>**, **<span style="color:#0056b3;">audit trails</span>**).

- **<span style="color:#0056b3;">Wallet Interoperability</span>** – A **standardized `navigator.utxo` API** for browsers and a **wallet‑web messaging protocol** (`window.postMessage`), enabling seamless interaction between web applications and wallets across **<span style="color:#0056b3;">crypto and fiat rails</span>**.

- **<span style="color:#0056b3;">Regulatory Alignment</span>** – Full alignment with major global stablecoin and digital asset regulatory frameworks, including **<span style="color:#0056b3;">EU MiCA</span>** (fully effective in 2025), **<span style="color:#0056b3;">Hong Kong Stablecoins Ordinance (Cap. 656)</span>**, **<span style="color:#0056b3;">US GENIUS Act</span>**, **<span style="color:#0056b3;">Singapore MAS stablecoin framework</span>**, and **<span style="color:#0056b3;">Japan's Payment Services Act</span>** amendments, ensuring wallet interoperability **<span style="color:#0056b3;">does not compromise compliance</span>** across jurisdictions.

---

🔗 **Key Resources:**
- [Charter](./charter/charter.md) – Group scope and deliverables
- [Reference Implementation](https://github.com/cocafoundation6/utxo6-dns) – UTXO6-DNS protocol implementation
- [IETF Draft](https://www.ietf.org/ietf-ftp/internet-drafts/draft-guorong-utxo-dns-01.html) – UTXO-DNS wire protocol standardization
- [Join the CG](https://www.w3.org/community/uw2i/join) – Participate in the community group

## Status

This is a **Work in Progress (WIP)**. The group is collecting implementation experience.

The community will consider as input to its work **[`draft-guorong-utxo-dns-01`](https://www.ietf.org/ietf-ftp/internet-drafts/draft-guorong-utxo-dns-01.html)** (UTXO6-DNS: Base Protocol and PRN Regulatory Extensions).

Reference implementation: **[UTXO6-DNS v0.1.0](https://github.com/cocafoundation6/utxo6-dns/releases/tag/v0.1.0)**


## Repository Structure
- `spec/` - Specification source files (Bikeshed & HTML)
- `explainers/` - Supplementary materials (Charter, Use Cases)
- `api/` - WebIDL definitions


## Contributing
Feedback and pull requests are welcome. Please refer to the [Contribution Guidelines](CONTRIBUTING.md).

docs: fix IETF draft link and add reference implementation link
docs: add W3C（World Wide Web Consortium）consortium brand image to README (width: 150)
docs: update README header with bold+blue key terms for global compliance

- Highlight core fields: crypto-fiat compliance, stablecoins, vLEI, KYC/AML
- Expand regulatory alignment to global frameworks
- Add blue color to key terms for visual emphasis
