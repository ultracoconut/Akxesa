## Manager API

**Purpose:**  
Manages subscriptions, account authorization, plans, and access verification for a single SaaS product.

### 1. Core Functions (State-Changing)

| Function | Inputs | Outputs | Permissions | Description |
|---------|--------|---------|-------------|-------------|
| createSubscription | `address owner`, `uint256 planId`, `uint256 duration` | — | Issuer only | Creates a new subscription for the owner. |
| extendSubscription | `address owner`, `uint256 extraDuration` | — | Issuer only | Extends an existing subscription. |
| authorizeAccount | `address owner`, `address account` | — | Issuer only | Authorizes a secondary account. |
| revokeAccount | `address owner`, `address account` | — | Issuer only | Revokes a secondary account and consumes a modification. |
| changeIssuer | `address newIssuer` | — | Issuer only | Updates the issuer. |
| changePlan | `address owner`, `uint256 newPlanId` | — | Issuer only | Changes plan for an existing subscription. |

### 2. Read-Only Functions

| Function | Inputs | Outputs | Permissions | Description |
|---------|--------|---------|-------------|-------------|
| getAccess | `address account` | `bool hasAccess`, `uint256 expiresAt`, `uint256 planId`, `bool isOwner` | Anyone | Returns access status, expiration timestamp, plan ID, and whether the account is the subscription owner. Secondary accounts inherit access from their linked owner. |  
| getSecondaryAccounts | `address owner` | `address[]` | Anyone | Returns linked secondary accounts. |
| hasFreeSlot | `address owner` | `bool` | Anyone | Checks if more secondary accounts can be added. |

### 3. Auto-Generated Getters (Public State Variables)

| Function | Inputs | Outputs | Permissions | Description |
|---------|--------|---------|-------------|-------------|
| issuer | — | `address` | Anyone | Returns the current issuer. |
| defaultDuration | — | `uint256` | Anyone | Returns default subscription duration. |
| maxSecondaryAccounts | — | `uint256` | Anyone | Max secondary accounts per subscription. |
| maxModifications | — | `uint256` | Anyone | Max revocations allowed. |
| maxSubscriptions | — | `uint256` | Anyone | Max subscriptions allowed. |
| totalSubscriptions | — | `uint256` | Anyone | Total subscriptions created. |
| expiresAt | `address owner` | `uint256` | Anyone | Subscription expiration timestamp. |
| planId | `address owner` | `uint256` | Anyone | Active plan ID. |
| modificationCount | `address owner` | `uint256` | Anyone | Revocations used. |
| isOwner | `address account` | `bool` | Anyone | Whether account is an owner. |
| linkedToOwner | `address account` | `address` | Anyone | Owner linked to secondary account. |


### 4. Events

| Event | Parameters | Description |
|------|-----------|-------------|
| SubscriptionCreated | `address owner`, `uint256 planId`, `uint256 expiresAt` | Emitted when a new subscription is created for an owner with a specific plan and expiration timestamp. |
| SubscriptionExtended | `address owner`, `uint256 newExpiresAt` | Emitted when an existing subscription is extended and a new expiration timestamp is set. |
| AccountAuthorized | `address owner`, `address account` | Emitted when a secondary account is authorized under an owner's subscription. |
| AccountRevoked | `address owner`, `address account` | Emitted when a secondary account is revoked from an owner's subscription. |
| IssuerChanged | `address newIssuer` | Emitted when the issuer of the SubscriptionManager is changed. |
| PlanChanged | `address owner`, `uint256 oldPlanId`, `uint256 newPlanId` | Emitted when a subscription changes its plan. |

