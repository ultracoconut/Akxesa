----Unfinished------
# Kusama Experiment (Akxesa)

## Disclaimer
⚠️ Experimental deployment on Kusama.
- This is experimental software provided as-is.
- No guarantees, no SLAs, no liability.
- No responsibility is assumed for loss or damages.
- Not intended for production or commercial use.

## Overview
This document describes how to experiment with SUMO
using the limited Factory deployment on Kusama.
Only 20 Subscription Managers are available. First come, first served.

## How to claim a Manager
  
### 1. Configure MetaMask
Configure MetaMask to connect to Kusama Asset Hub (EVM).
In Metamask go to Networks and Click in Add Custom Network, fill it with this network data:

Network Name: Kusama Asset Hub (EVM)
Default RPC URL: https://kusama-asset-hub-eth-rpc.polkadot.io 
Chain ID:  420420418
Currency symbol: KSM 
Block explorer URL: https://assethub-kusama.subscan.io/evm_transaction

#### 2. Open Remix
Go to https://remix.polkadot.io/. and connect MetaMask.

#### 3. Add .abi files to your remix directory
Add `Factory.abi` and `Manager.abi` to your Remix directory

#### 4. Attach the Factory contract
In Remix, select `Factory.abi` in your directory

Select Deploy & run transactions in the left menu, use **"At Address"** and attach this contract (v1.1) address:  
`0x`

Now you can access the contract methods.

#### 5. Create your Subscription Manager
Call `createSubscriptionManager`, for example:

- `issuer`: `Address authorized` to operate the manager
- `defaultDuration`: `600` (10 minutes)
- `maxSecondaryAccounts`: `5`
- `maxModifications`: `10`

This deploys a **brand-new, independent SubscriptionManager contract**.  
The provided `issuer` address is **granted exclusive permission to manage subscriptions**

Expect chaos


