# Akxesa - The source of truth for SaaS access

## About Akxesa

**Akxesa** is an on-chain subscription and licensing system for SaaS platforms.

It provides verifiable, decentralized access control using smart contracts, while remaining compatible with both Web3-native and traditional Web2 SaaS products.

Akxesa is built on **Polkadot Asset Hub** and is currently being tested on **Paseo Asset Hub (testnet)**.  

## 🎯 UX & Design Philosophy

Akxesa is designed for real-world SaaS adoption, not just Web3-native users.

The system intentionally removes common blockchain UX barriers while preserving on-chain guarantees.  
To enable a broader reach and a better user experience:

- End users do **not** need to hold tokens
- End users do **not** need to sign transactions
- No gas payments are required from users

All on-chain operations are executed by the SaaS service itself, which acts as the issuer and covers gas costs when interacting with the smart contracts.

**This model preserves:**
- On-chain verifiability
- Transparency and auditability
- Deterministic access control

**While removing:**
- Wallet friction
- Gas management
- Web3 onboarding complexity

## 🔑 Account Model

Akxesa smart contracts operate exclusively on `EVM address (H160)`.

The contracts themselves only verify access rights for a given on-chain address.

Any identity model (Web3 wallets, Substrate accounts, Web2 users, custodial accounts, or external systems) can be supported by the application layer, as long as it resolves to an EVM-compatible address when interacting with the contracts.

This separation keeps Akxesa contracts minimal, auditable, and chain-native, while allowing applications full flexibility over identity and UX design.  

## 🧠 High-level architecture

- Each SaaS deploys **one SubscriptionManager**, created via the Factory
- The Factory deploys managers dynamically using embedded bytecode
- Each manager is fully independent and immutable
- An **issuer account** (typically the SaaS backend or operator) controls all subscription state
- End users never sign transactions

## 🧩 Roles

### Issuer

The issuer is the authority for a given `SubscriptionManager`.

Typically:

- Backend SaaS service
- SaaS Operator wallet

The issuer can:

- Create subscriptions
- Extend subscriptions
- Authorize or revoke secondary accounts
- Change the issuer address


### Owner

Each subscription has an `owner` account:

- Identifies the primary account of the subscription
- Automatically authorized

### Secondary Account

Additional accounts linked to a subscription.


## 🏭 Subscription Manager Factory

- Deploys a brand-new `SubscriptionManager`
- Emits `ManagerCreated`
- Returns the address of the new manager
- Each manager is fully independent and owned by its issuer


## 📦 Subscription Manager

A Subscription Manager is an independent on-chain access control system.  
It manages:
- Subscription ownership
- Secondary account authorization
- Plan assignment
- Expiration logic

Each manager operates in isolation and enforces its own limits defined at creation time.

## 🔐 Subscription Model

- Each subscription is owned by a primary account
- Additional accounts can be linked to the subscription
- All access rules are enforced on-chain
- Applications only need to perform read-only checks


## 🔢 System Limits (per manager)

| Concept                 | Meaning                                           | Configurable |
|-------------------------|--------------------------------------------------|--------------|
| Subscriptions per manager | Maximum subscriptions allowed by issuer | No configurable (50,000) |
| Secondary accounts per subscription | Additional non-owner accounts that can access the same subscription | Configurable (≤5) |
| Modifications | Number of secondary account revocations | Configurable (≤20) |
| Subscription per account | An account can belong to only one subscription  | Enforced |

> Limits are defined at manager creation time via the Factory.


## 🚀 Basic Flow

### 1. Create SubscriptionManager (anyone)

Any account can call the Factory to deploy a new SubscriptionManager:

```solidity
createSubscriptionManager(
        address issuer,
        uint256 defaultDuration,
        uint256 maxSecondaryAccounts,
        uint256 maxModifications
    )
```
Deploys a brand-new SubscriptionManager with custom limits and defaults.

- issuer - The account that will be authorized to manage subscriptions.
  
- defaultDuration - Default subscription duration (in seconds)
  
- maxSecondaryAccounts - Maximum secondary (non-owner) accounts per subscription

- maxModifications - Maximum number of account revocations allowed per subscription


### 2. Create subscription (issuer)

```solidity
createSubscription(address owner, uint256 planId, uint256 duration)
```
- Creates a new subscription

- Owner is automatically authorized

- duration is expressed in seconds


### 3. Manage accounts (issuer)

```solidity
authorizeAccount(address owner, address account)
revokeAccount(address owner, address account)
```

All account management is performed by the issuer.

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

## 📣 Events

### Factory

```solidity
ManagerCreated(address manager, address issuer)
```

### SubscriptionManager

```solidity
SubscriptionCreated(address owner, uint256 planId, uint256 expiresAt)
SubscriptionExtended(address owner, uint256 newExpiresAt)
AccountAuthorized(address owner, address account)
AccountRevoked(address owner, address account)
IssuerChanged(address newIssuer)
```

Applications can index these events to sync off-chain state.


## 🛡️ Security Model

- Only the issuer can mutate state

- No personal data stored on-chain

- Fully auditable and deterministic

- Immutable once deployed


## 🌍 Use Cases

- Web3-native SaaS subscriptions

- Software licensing

- Multi-device access control


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


## 📜 License

Copyright © 2026 @Ultracoconut. All rights reserved.

Unauthorized copying, use, modification, distribution, or disclosure of this
software, in whole or in part, is strictly prohibited without prior written
permission from the copyright holder.
