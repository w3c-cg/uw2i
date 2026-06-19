# UTXO Web Wallet Interoperability Community Group (UW2ICG) 

This repository hosts the [UTXO Web Wallet Interoperability Community Group](https://www.w3.org/groups/cg/uw2i/) work related to the integration of **UTXO-DNS** (`.utxo` domains) into the Web platform. It defines APIs to bridge **IETF UTXO6-DNS resolution** with Web applications, enabling seamless:

- **Cryptocurrency Payments** via Web Payments API extension (`utxo-payment`)
- **Decentralised Identity (DID)** via the `did:utxo` method
- **Wallet Interoperability** through the `navigator.utxo` API

## Status
This is a **Work in Progress (WIP)**. The group is collecting implementation experience.

The community will consider as input to its work [draft-guorong-utxo6-dns-base-00](https://datatracker.ietf.org/submit/status/161673/confirm/7364e157d123d1f4be78a5abb17e3506/).

## Repository Structure
- `spec/` - Specification source files (Bikeshed & HTML)
- `explainers/` - Supplementary materials (Charter, Use Cases)
- `api/` - WebIDL definitions

## Contributing
Feedback and pull requests are welcome. Please refer to the [Contribution Guidelines](CONTRIBUTING.md).
