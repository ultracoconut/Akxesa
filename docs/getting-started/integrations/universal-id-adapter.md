# Akxesa Universal ID Adapter

Author: @Ultracoconut  
License: MIT  
Note: This is example/demo code. It may contain bugs and is not production-ready. Use at your own risk

### Overview

This script deterministically maps users from **any authentication provider** to Akxesa on-chain addresses.

- Works with **Auth0, Firebase, Okta, Keycloak, Google, GitHub**, or any system providing a unique user ID.  
- Users get a **deterministic H160 address** (Ethereum-style) as their Akxesa owner address.  
- No storage is required; addresses can be derived on-the-fly.  
- Fully compatible with Akxesa Subscription Manager.

### Architecture

```text
Authentication Provider
        │
        ▼
  Unique User ID (Auth0 user_id, Firebase uid, etc.)
        │
        ▼
 keccak256 hash → last 20 bytes → H160 Akxesa Address
        │
        ▼
  Akxesa Identity
```

### Features

- Provider-agnostic – Works with any login system that exposes a unique identifier.

- Deterministic – Same user always gets the same Akxesa address.

- No storage needed – Eliminates the need for database or metadata.

- These addresses are H160 abstract identifiers, not EOAs. No private keys exist or are expected


### Usage

```js
/**
 * Generate a deterministic Akxesa address from a unique user ID.
 * @param {string} uniqueId - Unique identifier from any auth provider
 * @returns {string} - Deterministic H160 address
 */

const { keccak256, toUtf8Bytes, getAddress } = require("ethers");

function akxesaAddressFromId(uniqueId) {
  const hash = keccak256(toUtf8Bytes(uniqueId));
  const address = "0x" + hash.slice(-40); // last 20 bytes
  return getAddress(address);
}

// Example usage:
const exampleUserIds = [
  "auth0|123456789",
  "google-oauth2|987654321",
  "firebase|uid_ABC123",
  "okta|sub_456DEF"
];

exampleUserIds.forEach(id => {
  console.log(`Akxesa identity for ${id} → ${akxesaAddressFromId(id)}`);
});
```

### Notes

- Fully compatible with Akxesa Subscription Manager
