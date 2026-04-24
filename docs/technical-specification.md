
# Technical Specification

This document describes the core architecture, account model, and system limits of Akxesa.

## Architecture

- Each SaaS gets its own isolated subscription system (SubscriptionManager)  
- Managers are deployed via a factory and are immutable  
- The backend (issuer) controls all subscription state  
- End users never interact with the system directly

## Account Model

Akxesa contracts operate on EVM addresses (H160).

Any identity system (Auth0, Firebase, custom backends, etc.) can be used, as long as it resolves deterministically to an address.

The **Akxesa Universal ID Adapter** allows deriving an address from a stable user identifier.

This keeps contracts minimal and allows full flexibility at the application layer.


## System Limits

These limits define how each SubscriptionManager behaves at creation time and are enforced on-chain.

| Concept                 | Meaning                                           | Configurable |
|-------------------------|--------------------------------------------------|--------------|
| Subscriptions per manager | Maximum subscriptions allowed by issuer | Fixed (50,000) |
| Secondary accounts per subscription | Additional non-owner accounts that can access the same subscription | Configurable (≤5) |
| Modifications | Number of secondary account revocations | Configurable (≤20) |
| Subscription per account | An account can belong to only one subscription  | Enforced |

> Limits are defined at manager creation time via the Factory.
