
# Specification 'uw2i'

# UTXO-DNS Web Integration

**Group:** UTXO Web Wallet Interoperability Community Group (UW2ICG)  
**Charter:** [https://www.w3.org/community/utxo-web-wallet/](https://www.w3.org/community/utxo-web-wallet/)  
**Editors:** Guorong Tian (CoCa Foundation), Shiwen Tian (UW2ICG), Yidan Zhu (UW2ICG)  

This repository hosts the W3C Community Group Draft for integrating **UTXO-DNS** (`.utxo` domains) into the Web platform. It defines standardised APIs to bridge **IETF UTXO6-DNS resolution** with Web applications, enabling seamless:

- **Cryptocurrency Payments** via Web Payments API extension (`utxo-payment`)
- **Decentralised Identity (DID)** via the `did:utxo` method
- **Wallet Interoperability** through the `navigator.utxo` API

## Status
This is a **Work in Progress (WIP)**. The group is collecting implementation experience.

- Latest Editor's Draft: [https://uw2icg.github.io/utxo-web-integration/](https://uw2icg.github.io/utxo-web-integration/)
- IETF Base Spec: [draft-guorong-utxo6-dns-base-00](https://datatracker.ietf.org/doc/draft-guorong-utxo6-dns-base/)

## Repository Structure
- `spec/` - Specification source files (Bikeshed & HTML)
- `explainers/` - Supplementary materials (Charter, Use Cases)
- `api/` - WebIDL definitions

## Contributing
Feedback and pull requests are welcome. Please refer to the [Contribution Guidelines](CONTRIBUTING.md).
```

---

### 2. `spec/index.html`


```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>UTXO-DNS Web Integration</title>
  <style>
    /* W3C Base Style */
    body { max-width: 780px; margin: 0 auto; padding: 2em 1em; font: 16px/1.5 system-ui, sans-serif; color: #222; background: #fff; }
    h1, h2, h3 { color: #005A9C; }
    dt { font-weight: bold; margin-top: 1em; }
    dd { margin-left: 1.5em; }
    .note { background: #f4f8ff; border-left: 4px solid #005A9C; padding: 0.5em 1em; }
    pre { background: #f0f0f0; padding: 1em; overflow-x: auto; border-radius: 4px; }
    code { font-size: 0.9em; }
    table { width: 100%; border-collapse: collapse; margin: 1em 0; }
    th, td { border: 1px solid #ccc; padding: 0.5em; text-align: left; }
    th { background: #f4f4f4; }
  </style>
</head>
<body>

<p><a href="https://www.w3.org/community/utxo-web-wallet/">🔗 UW2ICG</a> &mdash; Editor's Draft</p>

<h1>UTXO-DNS Web Integration<br><small>Integrating Blockchain Domain Resolution into the Web Platform</small></h1>

<p><strong>Version:</strong> 0.2 (CG Draft)<br>
<strong>Date:</strong> 19 June 2026<br>
<strong>Editor:</strong> Guorong Tian (CoCa Global Joint Foundation / UW2ICG) &lt;foundation@utxocp.cn&gt;<br>
<strong>Contributors:</strong> W3C Web Payments WG, W3C DID WG, UW2ICG, CoCa Foundation</p>

<div class="note">
  <strong>Status of This Document:</strong> This is a <strong>W3C Community Group Draft</strong>. It has not yet been endorsed by the W3C Membership. Implementation experience is encouraged but production deployment is not recommended at this stage.
</div>

<h2>Abstract</h2>
<p>This document specifies how to integrate <strong>IETF UTXO6-DNS</strong> resolution (`.utxo` domains to P2P Endpoint Identifiers) into the Web platform. It defines APIs for Web applications to resolve `.utxo` domains into blockchain addresses or DIDs, extends the Payment Request API with a <code>utxo-payment</code> method, and defines the <code>did:utxo</code> method for decentralised identity.</p>

<h2>Table of Contents</h2>
<ul>
  <li><a href="#intro">1. Introduction</a></li>
  <li><a href="#dependencies">2. Dependencies</a></li>
  <li><a href="#terminology">3. Terminology</a></li>
  <li><a href="#architecture">4. Integration Architecture</a></li>
  <li><a href="#api">5. Web API Extensions</a></li>
  <li><a href="#payment">6. Payment Integration</a></li>
  <li><a href="#did">7. DID Integration</a></li>
  <li><a href="#wallet">8. Wallet Recommendations</a></li>
  <li><a href="#security">9. Security Considerations</a></li>
</ul>

<h2 id="intro">1. Introduction</h2>
<p>The IETF UTXO6-DNS specification defines a resolution protocol that maps `.utxo` domain names to <strong>P2P Endpoint Identifiers (PEIs)</strong>. However, the Web platform lacks standardised ways to consume these resolution results. This document fills that gap by defining:</p>
<ul>
  <li>A JavaScript API (<code>resolveUtxoDomain</code>) for Web applications.</li>
  <li>An extension to the <strong>Payment Request API</strong> for `.utxo` domain payments.</li>
  <li>A new DID method (<code>did:utxo</code>).</li>
</ul>

<h2 id="dependencies">2. Dependencies</h2>
<ul>
  <li><strong>IETF UTXO6-DNS</strong> (<code>draft-guorong-utxo6-dns-base-00</code>)</li>
  <li><strong>W3C Payment Request API</strong> (<a href="https://www.w3.org/TR/payment-request/">https://www.w3.org/TR/payment-request/</a>)</li>
  <li><strong>W3C DID Core</strong> (<a href="https://www.w3.org/TR/did-core/">https://www.w3.org/TR/did-core/</a>)</li>
  <li><strong>W3C Verifiable Credentials</strong></li>
</ul>

<h2 id="terminology">3. Terminology</h2>
<dl>
  <dt>PEI</dt><dd>P2P Endpoint Identifier – the resolved result from UTXO6-DNS.</dd>
  <dt>`.utxo` domain</dt><dd>A domain name ending with the <code>.utxo</code> top-level label.</dd>
  <dt>UTXO Resolver</dt><dd>Client component implementing the API defined in this document.</dd>
</dl>

<h2 id="architecture">4. Integration Architecture</h2>
<pre>
┌─────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Web App  │───▶│  UTXO Resolver    │───▶│  UTXO6-DNS     │
│  (Payment)  │     │  (API)            │     │  (Recursive)      │
└─────────────┘     └─────────────────┘     └────────┬───────┘
                                                      │
                                                     ▼
                                             ┌─────────────┐
                                             │ Blockchain    │
                                             │ (UTXO NFT)   │
                                             └─────────────┘
</pre>

<h2 id="api">5. Web API Extensions</h2>
<h3>5.1 <code>resolveUtxoDomain()</code></h3>
<pre>
partial interface Navigator {
  [SecureContext, SameObject] readonly attribute UtxoResolver utxo;
};

interface UtxoResolver {
  Promise&lt;PEIRecord&gt; resolveUtxoDomain(USVString domain, optional UtxoResolveOptions options);
};
</pre>

<h3>5.2 <code>PEIRecord</code> Interface</h3>
<pre>
dictionary PEIRecord {
  required USVString domain;
  required unsigned long chainId;
  required octet peiType;
  required ArrayBuffer pei;
  DOMString? ipv6Address;
  DOMString? blockchainAddress;
  DOMString? didUrl;
  ArrayBuffer? merkleProof;
  DOMHighResTimeStamp expires;
};
</pre>

<h2 id="payment">6. Payment Integration (Web Payments API)</h2>
<h3>6.1 Extended <code>PaymentMethodData</code></h3>
<pre>
dictionary UtxoPaymentData {
  required USVString domain;
  USVString? expectedChainId;
  USVString? expectedCurrency;
  boolean requireProof = false;
};
</pre>
<p><strong>Example:</strong></p>
<pre>
{
  "supportedMethods": "utxo-payment",
  "data": {
    "domain": "merchant.utxo",
    "expectedChainId": "2",
    "expectedCurrency": "USDC"
  }
}
</pre>

<h2 id="did">7. DID Integration</h2>
<h3>7.1 <code>did:utxo</code> Method</h3>
<p>Syntax: <code>did:utxo:&lt;domain&gt;</code> (e.g., <code>did:utxo:alice.utxo</code>).</p>
<p>The resolution algorithm:</p>
<ol>
  <li>Extract the domain part.</li>
  <li>Call <code>resolveUtxoDomain(domain, { expectedPeiType: 5 })</code>.</li>
  <li>If a PEI of type 5 (DIDURL) exists, resolve that DID.</li>
  <li>Otherwise, construct a minimal DID document dynamically.</li>
</ol>
<p><strong>Example DID Document:</strong></p>
<pre>
{
  "@context": ["https://www.w3.org/ns/did/v1"],
  "id": "did:utxo:example.utxo",
  "verificationMethod": [{
    "id": "#controller",
    "type": "EcdsaSecp256k1VerificationKey2019",
    "controller": "did:utxo:example.utxo",
    "blockchainAccountId": "eip155:1:0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb0"
  }],
  "authentication": ["#controller"]
}
</pre>

<h2 id="wallet">8. Wallet and Browser Integration Recommendations</h2>
<ul>
  <li><strong>Native support:</strong> Browsers should implement <code>navigator.utxo</code>.</li>
  <li><strong>Fallback:</strong> Applications should use a server-side proxy if the API is unavailable.</li>
  <li><strong>Multi-chain:</strong> Support Bitcoin (1), Ethereum (2), Solana (4), and JMBC (3).</li>
  <li><strong>Stablecoins:</strong> Support USDC, USDT, and Hong Kong regulated stablecoins.</li>
</ul>

<h2 id="security">9. Security Considerations</h2>
<ul>
  <li><strong>Domain spoofing:</strong> Verify Merkle proofs when <code>requireProof</code> is <code>true</code>.</li>
  <li><strong>Private key handling:</strong> Wallets must never expose private keys to Web applications.</li>
  <li><strong>Payment confirmation:</strong> User agents must display the recipient domain and amount prominently.</li>
</ul>

<hr>
<p><strong>Copyright Notice</strong><br>
© 2026 UW2ICG Contributors. This document is licensed under the W3C Community Contributor License Agreement.</p>

</body>
</html>
```
