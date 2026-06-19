
# Specification 'uw2i'

# UTXO-DNS Web Integration

**Group:** UTXO Web Wallet Interoperability Community Group (UW2ICG)  
**Charter:** TBD
**Editors:** Guorong Tian (CoCa Foundation), Shiwen Tian (UW2ICG), Yidan Zhu (UW2ICG)  

This repository hosts the W3C Community Group Draft for integrating **UTXO-DNS** (`.utxo` domains) into the Web platform. It defines standardised APIs to bridge **IETF UTXO6-DNS resolution** with Web applications, enabling seamless:

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
