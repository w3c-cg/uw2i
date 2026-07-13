# UTXO Web Wallet Interoperability Community Group (UW2ICG) 

This repository hosts the [UTXO Web Wallet Interoperability Community Group](https://www.w3.org/groups/cg/uw2i/) work related to the integration of **UTXO-DNS** (`.utxo` domains) into the Web platform. It defines APIs to bridge **IETF UTXO6-DNS resolution** with Web applications, enabling seamless:

- **Cryptocurrency Payments** via Web Payments API extension (`utxo-payment`)
- **Decentralised Identity (DID)** via the `did:utxo` method
- **Wallet Interoperability** through the `navigator.utxo` API

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
