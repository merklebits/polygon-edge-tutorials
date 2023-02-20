# Setting up Polygon Edge with POS

## Overview

For an introduction to POS, you can refer to [this](https://wiki.polygon.technology/docs/edge/consensus/pos-concepts/) article. Setting Polygon Edge with POS is quite similar to doing it via [POA](https://github.com/nonceblox/polygon-edge-tutorials/blob/master/setup_local.md)

## 1. Starting the network

Steps `1` and `2` remain identical to the POA. We Initialize data folders for IBFT and generate validator keys. These keys are used for preparing multiaddr conncection strings for the bootnode. 

It is in Step 3 that we introduce a new `--pos` flag when creating the `genesis.json` file.
```
polygon-edge genesis --pos --epoch-size 50 ...
``` 

The `--pos` flag also pre-deploys the [Staking Smart Contract](https://github.com/0xPolygon/staking-contracts/blob/main/contracts/Staking.sol) on the address `0x0000000000000000000000000000000000001001` from block 0.

The ``epoch-size`` flag sets the frequency with which the validator set updates. At 50, the validator set wwill be updated every 100 seconds.

## 2. Setting up the Staking Smart Contract

The staking smart contract repo is a hardhat project, which requires NPM.

```
git clone https://github.com/0xPolygon/staking-contracts
cd staking-contracts
npm install
vim .env
```

In the `.env` file add the following parameters:
```
JSONRPC_URL=http://localhost:10002
PRIVATE_KEYS=0x0454f3ec51e7d6971fc345998bb2ba483a8d9d30d46ad890434e6f88ecb97544
STAKING_CONTRACT_ADDRESS=0x0000000000000000000000000000000000001001
```

## 3. Staking
Next, stake with the following command:
```
npm run stake
```
The `stake` Hardhat script stakes a default amount of `1 ETH`, which can be changed by modifying the `scripts/stake.ts` file.

If the funds being staked are `>=1 ETH`, the validator set on the Staking Smart Contract is updated, and the address will be a part of the validator set next epoch onwards.

## 4. Unstaking
Address that have a stake can only unstake all of their funds at once
```
npm run unstake
```

## 5. Fetching the list of stakers
All addresses that stake funds are saved to the Staking Smart Contract.
```
npm run info
```
