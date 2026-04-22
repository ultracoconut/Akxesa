# Akxesa - The source of truth for SaaS access

Sell SaaS subscriptions with verifiable access control.

Akxesa lets you eliminate your subscription database and verify access on-chain while keeping your existing backend architectures and authentication providers.

## ⚡  Quickstart (5 minutes)

1. Create your SubscriptionManager → https://www.akxesa.com/app  
2. Derive a deterministic address from your user ID (Auth0, Firebase, etc.)
3. Call `getAccess(address)` from your backend  

That’s it. No subscription database required.

## 📖 About Akxesa

Akxesa is a deterministic subscription and licensing system designed for SaaS platforms.

It provides verifiable access control using smart contracts, while remaining fully compatible with traditional authentication systems and backend architectures.

Akxesa is built on Polkadot Asset Hub and currently runs on Paseo Asset Hub (testnet).


## 🎯 UX & Design Philosophy

Akxesa is designed for real-world SaaS adoption.

End users do not need:
- wallets  
- tokens  
- transaction signing  

All operations are handled by the SaaS backend (issuer), which interacts with the contracts and covers execution costs.

This preserves:
- Deterministic access control  
- Auditability  
- Verifiable state  

While removing:
- User friction  
- Wallet dependency  
- Gas management complexity  

## 🔑 Account Model

Akxesa contracts operate on EVM addresses (H160).

Any identity system (Auth0, Firebase, custom backends, etc.) can be used, as long as it resolves deterministically to an address.

The **Akxesa Universal ID Adapter** allows deriving an address from a stable user identifier.

This keeps contracts minimal and allows full flexibility at the application layer.


## 🧠 Architecture

- Each SaaS gets its own isolated subscription system (SubscriptionManager)  
- Managers are deployed via a factory and are immutable  
- The backend (issuer) controls all subscription state  
- End users never interact with the system directly
  
## 🧩 Roles

### Issuer (SaaS backend)
Controls subscription state:
- Create subscriptions  
- Extend subscriptions  
- Manage access  

### Subscriber (owner)
Primary account of a subscription.

### Linked Accounts
Additional accounts with shared access.


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

### 2. Create subscription

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
- Multi-device access (e.g. 5 seats per account)  
- Shared subscription plans  

## ▶️ Getting Started

### Create your SubscriptionManager
Create and configure your own independent SubscriptionManager using the Akxesa app:

👉 https://www.akxesa.com/app

### Backend integration
Learn how to integrate Akxesa into your SaaS backend:

👉 [Backend Integration Guide](docs/getting-started/backend-integration.md)

### Manual interaction (advanced)
Interact directly with the contracts using Remix and MetaMask:

👉 [Manual Interaction Guide](docs/getting-started/manual-interaction.md)


## 📚 API Documentation
Complete smart contract interface reference
- [Factory API](docs/api/factory-api.md)
- [Manager API](docs/api/manager-api.md)
- [Error Reference](docs/api/errors.md)


## 🔢 System Limits

These limits define how each SubscriptionManager behaves at creation time and are enforced on-chain.

| Concept                 | Meaning                                           | Configurable |
|-------------------------|--------------------------------------------------|--------------|
| Subscriptions per manager | Maximum subscriptions allowed by issuer | Fixed (50,000) |
| Secondary accounts per subscription | Additional non-owner accounts that can access the same subscription | Configurable (≤5) |
| Modifications | Number of secondary account revocations | Configurable (≤20) |
| Subscription per account | An account can belong to only one subscription  | Enforced |

> Limits are defined at manager creation time via the Factory.


## 📜 License

Copyright © 2026 @Ultracoconut. All rights reserved.

Unauthorized copying, use, modification, distribution, or disclosure of this
software, in whole or in part, is strictly prohibited without prior written
permission from the copyright holder.
