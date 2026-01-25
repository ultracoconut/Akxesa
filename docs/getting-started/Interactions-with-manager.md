# Usage Examples

### Requirements
- Node.js **v18+**
- `ethers` v6
```bash
npm install ethers
```
### Verify subscription access

```javascript
import { ethers } from "ethers";
import ManagerABI from "./Manager.json" assert { type: "json" };

// Paseo Asset Hub EVM RPC
const provider = new ethers.JsonRpcProvider(
  "https://testnet-passet-hub-eth-rpc.polkadot.io"
);

// Deployed SubscriptionManager address
const MANAGER_ADDRESS = "0xYourManagerAddressHere";

// Account to check access for
const userAddress = "0xUserAddressHere";

async function checkAccess() {
  const manager = new ethers.Contract(
    MANAGER_ADDRESS,
    ManagerABI,
    provider
  );

  const access = await manager.getAccess(userAddress);

  console.log(access);
}

checkAccess();
```

**Example response**

```javascript
{
  hasAccess: true,
  planId: 1n,
  expiresAt: 1712345678n,
  isOwner: false
}
```

Numeric values are returned as `BigInt` when using ethers v6.

**Notes**
- This is a read-only call 
- No signer or private key is required 
- No gas is consumed 
- All `SubscriptionManager` instances share the same ABI — only the contract address changes


### Create a Subscription

```javascript
import { ethers } from "ethers";
import ManagerABI from "./Manager.json" assert { type: "json" };

// Paseo Asset Hub EVM RPC
const provider = new ethers.JsonRpcProvider(
  "https://testnet-passet-hub-eth-rpc.polkadot.io"
);

// Issuer private key
// Must correspond to the issuer address of the SubscriptionManager
const ISSUER_PRIVATE_KEY = "0xYOUR_PRIVATE_KEY_HERE";

// Deployed SubscriptionManager address
const MANAGER_ADDRESS = "0xYOUR_MANAGER_ADDRESS_HERE";

// Subscription owner (end user account)
const OWNER_ADDRESS = "0xSUBSCRIPTION_OWNER_ADDRESS";

const issuerSigner = new ethers.Wallet(
  ISSUER_PRIVATE_KEY,
  provider
);

async function createSubscription() {
  const manager = new ethers.Contract(
    MANAGER_ADDRESS,
    ManagerABI,
    issuerSigner
  );

  const planId = 1;     // arbitrary plan identifier
  const duration = 0;  // 0 = use defaultDuration defined at manager creation

  const tx = await manager.createSubscription(
    OWNER_ADDRESS,
    planId,
    duration
  );

  console.log("Transaction sent:", tx.hash);

  const receipt = await tx.wait();
  console.log("Subscription created in block:", receipt.blockNumber);
}

createSubscription().catch(console.error);

```
**Notes**
- Requires private key for signing
- State-changing transaction (gas required)
