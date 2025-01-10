# Withdrawal of Titan assets via Etherscan

## Introduction

When the Titan Network service is closed, the remaining assets on the Titan network can be withdrawn from the Ethereum network.

This document explains how to withdraw assets through Etherscan.

The [`titan_new-generate-assets.json`](https://github.com/tokamak-network/L2-Assets-Migration/blob/genStorage/titanLagacy/data/titan_new-generate-assets.json) document provides hashes that can be claimed for a specific asset.

## Steps

### Step 1. In the ["titan_new-generate-assets.json"](https://github.com/tokamak-network/L2-Assets-Migration/blob/genStorage/titanLagacy/data/titan_new-generate-assets.json) file, locate your address in the data section corresponding to your desired token.

**Note: The token names in the document correspond to the following assets:**

- ETH → "tokenName": "Ether"
- TON → "tokenName": "Tokamak Network Token"
- TOS → "tokenName": "TONStarter"
- USDC → "tokenName": "USD Coin"
- USDT → "tokenName": "Tether USD"

### Step 2. Copy the l1Token address, amount, and hash values corresponding to your address from the `generate-assets3.json` file.

If you look up the address 0x796C1f28c777b8a5851D356EBbc9DeC2ee51137F,
If you search below,

![image](/assets/forceclaim/image01.png)

- `claimer`: 0x796c1f28c777b8a5851d356ebbc9dec2ee51137f
- `1Token`: 0x000000000000000000000000000000000000000
- `amount`: 77075825179826438
- `hash`: 0xec4a8c41e7718aceeaeb44d7fdf766e0f0f4b1f25b0af5cc890daae348c35b57

**Note: Note: The amount is displayed in wei. To convert to the standard decimal format, divide by 10^18.**

### Step 3. Verify if your token is claimable by checking the claim state:

- Go to the L1StandardBridge contract on Etherscan ([link](https://etherscan.io/address/0x59aa194798Ba87D26Ba6bEF80B85ec465F4bbcfD#readProxyContract))
- Find the `claimState` function([link](https://www.notion.so/Withdrawal-of-assets-via-Etherscan-ENG-165d96a400a38029858fee0ee9a9eec7?pvs=4))
- Enter your hash value from Step 2
- Click "Query"
- Check the return value:
  - If `false`: The token is claimable
  - If `true`: The token has already been claimed and cannot be claimed again

### Step 4. Look up the contract position that stores the claim information.

- Contract: L1StandardBridge ([link](https://etherscan.io/address/0x59aa194798Ba87D26Ba6bEF80B85ec465F4bbcfD#readProxyContract))
- Function: getForcePosition
  - `_hash` (string): Hash value retrieved in Step 2
- Return (address): **the contract position address**

### Step 5. Claim

You can claim one asset or multiple assets at once.

- Connect your account into etherscan clicking `Connect to Web3`.

#### Step 5-1. Claiming a single asset

Go to the L1StandardBridge contract in ethersacn and click `Contract` → `Write as Proxy`.

- Contract: L1StandardBridge
- Function: `forceWithdrawClaim`
  - `_position` (address): The contract position address returned in step 4
  - `_hash` (string): hash retrieved in step 2
  - `_token` (address): l1Token retrieved in step 2
  - `_amount` (uint256): amount retrieved in step 2
  - `_address` (address): claimer retrieved in step 2

After entering the parameters, click the write button to execute the transaction.

![image](/assets/forceclaim/image02.png)

#### Step 5-2. Claiming multiple assets at once

Go to the L1StandardBridge([link](https://etherscan.io/address/0x59aa194798Ba87D26Ba6bEF80B85ec465F4bbcfD#readProxyContract)) contract in ethersacn and click `Contract` → `Write as Proxy`.

- Contract: L1StandardBridge

  ![image](/assets/forceclaim/image03.png)

  - Function: `forceWithdrawClaimBatch`
    - `params` :
      Enter each asset claim information as an array in the form of
      {address `position`, string `hashed`, address `token`, uint256 `amount`, address `getAddress`}
  - examples

    When claiming two assets, (1) TON and (2) ETH, enter as in (3).

    - (1) TON search results

      ```json
       "l1Token": "0xa30fe40285B8f5c0457DbC3B7C8A280373c40044",
      "l2Token": "0x7c6b91D9Be155A6Db01f749217d76fF02A7227F2",
      "tokenName": "Tokamak Network Token",

      "claimer": "0x757de9c340c556b56f62efae859da5e08baae7a2",
      "amount": "996000000000000000",
      "hash": "0x7d4ad43c4eb5152ab242a8eacacba53b20e06cff3a6533f6ce15cfcf03e2176d"


      getForcePosition:  0x1a7dFF905E30d36578b6b2aC46089dEc51531067
      ```

    - (2) ETH search results

      ```json
      "l1Token": "0x0000000000000000000000000000000000000000",
      "l2Token": "0xDeadDeAddeAddEAddeadDEaDDEAdDeaDDeAD0000",
      "tokenName": "Ether",

      "claimer": "0x757de9c340c556b56f62efae859da5e08baae7a2",
      "amount": "105899999967584534",
      "hash": "0x946fda2cf90bda4739dce26cf41c16776e462618b3752e9645a05bb5512e2781"


      getForcePosition:  0x1a7dFF905E30d36578b6b2aC46089dEc51531067
      ```

    - (3) Input values
      ```json
      [
        {
          "position": "0x1a7dFF905E30d36578b6b2aC46089dEc51531067",
          "hashed": "0x7d4ad43c4eb5152ab242a8eacacba53b20e06cff3a6533f6ce15cfcf03e2176d",
          "token": "0xa30fe40285B8f5c0457DbC3B7C8A280373c40044",
          "amount": "996000000000000000",
          "getAddress": "0x757de9c340c556b56f62efae859da5e08baae7a2"
        },
        {
          "position": "0x1a7dFF905E30d36578b6b2aC46089dEc51531067",
          "hashed": "0x946fda2cf90bda4739dce26cf41c16776e462618b3752e9645a05bb5512e2781",
          "token": "0x0000000000000000000000000000000000000000",
          "amount": "105899999967584534",
          "getAddress": "0x757de9c340c556b56f62efae859da5e08baae7a2"
        }
      ]
      ```
