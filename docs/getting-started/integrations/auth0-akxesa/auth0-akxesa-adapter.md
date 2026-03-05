# Auth0 → Akxesa Adapter

Author: @Ultracoconut  
License: MIT  
Note: This is example/demo code. It may contain bugs and is not production-ready. Use at your own risk

### Overview

This script automatically assigns Auth0 users an Akxesa on-chain address upon registration.

- Run this as an Auth0 **Post-User-Registration Hook**

- Auth0 users get a new random H160 address (Ethereum-style) as their Akxesa owner address

- Each user gets a unique address only once

### Features

- These addresses are H160 abstract identifiers, not EOAs. No private keys exist or are expected

- No blockchain interactions are performed here; the address is just registered

- Fully compatible with Akxesa Subscription Manager

### Usage

```js
const { randomBytes, hexlify, getAddress } = require("ethers");

exports.onExecutePostUserRegistration = async (event, api) => {
  const userId = event.user.user_id;

  // Generate Ethereum-like identifier
  const raw = hexlify(randomBytes(20));
  const akxesaAddress = getAddress(raw);

  // Persist mapping in app_metadata
  api.user.setAppMetadata("akxesa_address", akxesaAddress);

  console.log(`Akxesa identity created: Auth0 ID ${userId} → ${akxesaAddress}`);
};
```

### Notes:
- Use the `akxesa_address` in Auth0 `app_metadata` to interact with the Akxesa Subscription Manager
