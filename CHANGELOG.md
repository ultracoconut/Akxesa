# Changelog

## v1.1 — Clarity & Plan Management Update

###  New Features
- **Plan change support**
  - Added support for changing the active plan of an existing subscription without recreating it.
  - Enables plan upgrades or downgrades while preserving subscription continuity.

###  Behavior Improvements
- **`getAccess` clarity improvements**
  - Now returns `planId` and `expiresAt` even when the subscription is expired.
  - Access validity (`hasAccess`) is clearly separated from subscription metadata.
  - Improves off-chain indexing, analytics, and debugging.

- **`getSecondaryAccounts` consistency**
  - Secondary accounts are returned regardless of subscription expiration.
  - The function always returns the stored secondary accounts array (or an empty array).
  - Provides clearer and more predictable data for frontends and indexers.

###  Design Rationale
- Improved clarity and consistency between on-chain state and off-chain consumers.
- Clear separation between **subscription state** and **access rights**.
- No additional storage, counters, or gas overhead introduced.
