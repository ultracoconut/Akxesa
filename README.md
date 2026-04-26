# Akxesa - The source of truth for SaaS access

Sell SaaS subscriptions with verifiable access control.

Akxesa lets you eliminate your subscription database and verify access on-chain while keeping your existing backend architectures and authentication providers.

## ⚡  Quickstart (5 minutes)

1. Create your SubscriptionManager → https://www.akxesa.com/app  
2. Derive a deterministic address from your user ID (Auth0, Firebase, etc.) using [Akxesa Universal ID Adapter](docs/getting-started/integrations/universal-id-adapter.md)
3. Call `getAccess(address)` from your backend  

That’s it. No subscription database required.

## 📖 About Akxesa

Akxesa is a deterministic subscription and licensing system designed for SaaS platforms.

It provides verifiable access control using smart contracts, while remaining fully compatible with traditional authentication systems and backend architectures.

Akxesa is built on Polkadot Asset Hub and currently runs on Paseo Asset Hub (testnet).

## 🎯 Design Principles

Akxesa is designed for real-world SaaS adoption.

- No wallets, tokens, or user signatures  
- Fully compatible with existing auth systems  
- Backend-controlled (issuer handles all operations)  

Users never interact with the system directly.

This provides:
- Deterministic access control  
- Verifiable state  
- No user friction  
  
## 🧩 Roles

- **Issuer (backend)** → controls subscription state  
- **Subscriber (owner)** → primary account of a subscription  
- **Linked accounts** → additional accounts with shared access  

## 🚀 Basic Flow

### 1. Create SubscriptionManager
Deploy a manager via the Factory:

```solidity
createSubscriptionManager(
  address issuer,
  uint256 defaultDuration,
  uint256 maxSecondaryAccounts,
  uint256 maxModifications
)
```

### 2. Create subscription (issuer)

```solidity
createSubscription(address owner, uint256 planId, uint256 duration)
```

### 3. Manage accounts (issuer)

```solidity
authorizeAccount(address owner, address account)
revokeAccount(address owner, address account)
```

### 4. Verify access (application)

```solidity
getAccess(address account)
```

Returns:

- hasAccess - true / false

- planId - active plan

- expiresAt - expiration timestamp

- isOwner - whether the account is the owner  

  ✅ Read-only call  
  ✅ No gas  
  ✅ No wallet signature


## 🌍 Use Cases

- SaaS subscription management without a database  
- Software licensing without centralized control  
- Multi-device access (e.g. 5 seats per subscription)  
- Shared subscription plans  

## ▶️ Getting Started

### Create your SubscriptionManager
Create and configure your own independent SubscriptionManager using the Akxesa app:

👉 https://www.akxesa.com/app

### Backend integration
Learn how to integrate Akxesa into your SaaS backend:

👉 [Backend Integration Guide](docs/getting-started/integrations/backend-integration.md)

### Manual interaction (advanced)
Interact directly with the contracts using Remix and MetaMask:

👉 [Manual Interaction Guide](docs/getting-started/manual-interaction.md)

## 📚 API Documentation
Complete smart contract interface reference
- [Factory API](docs/api/factory-api.md)
- [Manager API](docs/api/manager-api.md)
- [Error Reference](docs/api/errors.md)

## 📄 Technical Specification

Learn how Akxesa works under the hood, including its architecture, account model, and system limits.

👉 [Technical Specification](docs/technical-specification.md)

## 📜 License

Copyright © 2026 @Ultracoconut. All rights reserved.

Unauthorized copying, use, modification, distribution, or disclosure of this
software, in whole or in part, is strictly prohibited without prior written
permission from the copyright holder.
