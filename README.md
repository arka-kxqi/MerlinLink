
# MerlinLink

## Overview
This repository contains the Solidity implementation for the **MerlinLink** protocol, developed for deployment on the Merlin Chain. The protocol focuses on enhancing cross-chain interactions and leveraging the unique features of the Merlin Chain ecosystem. See MerlinLink Docs for the design details of the protocol.

## Local Development
This repository includes MerlinLink's smart contracts in Solidity for deployment to the Merlin Chain. It also includes MerlinLink JS SDKs in the packages folder for integrating with other projects to interact with MerlinLink contracts.

To get started, run `yarn` to install the project dependencies. Since the contracts of MerlinLink need to be deployed on different chains, this project provides a script to switch the current chain. Before the first time of compilation, run `yarn hardhat chain --testnet ropsten` to initialize contracts for the Ropsten testnet. This command will copy the config file `MerlinConfig.sol` to the `contracts/utils` folder and set system invariants for the selected chain.

You can also run `yarn hardhat chain --testnet [id]` or `yarn hardhat chain --mainnet [id]` to switch the project to other chains. See `packages/presets/testnets.json` and `packages/presets/mainnets.json` for available chains and their respective IDs.

## Run Tests
Test cases are provided in the `tests` folder. Before running the tests, please run `yarn build:packages` to build MerlinLink SDKs, which are used by contract tests. Then, run `yarn test` to perform tests for MerlinLink contracts. Note that the test script will switch the current chain to the Ethereum testnet, as MerlinLink contract tests should be run under this condition.

The core MerlinLink SDK `@merlinlink/sdk` also provides its own test cases. Navigate to the `packages/sdk` folder and run `yarn test` to perform tests for it. SDK tests also require an Ethereum environment, so ensure the current chain is switched to Ethereum.

## Generate Documentation
Run `yarn docgen` to generate documentation for MerlinLink smart contracts, extracted from comments in the contract source code. The generated docs can be found in the `docs` folder.

## Estimate Gas Consumption
This project provides two scripts to estimate gas consumption for crucial methods of MerlinLink. Run `yarn hardhat estimate` to estimate the normal deployed MerlinLink contracts, and run `yarn hardhat estimate --upgradable true` to estimate gas when MerlinLink is deployed as an upgradable contract.

## Deployment
MerlinLink smart contracts can be deployed to multiple blockchains, either on their testnets or mainnets. You can see many scripts like `yarn testnet:[id]` and `yarn deploy:[id]` with different values of `id` (specifying the deploying chain) in `packages.json`. These scripts will switch the current chain for the project, compile the smart contract, and run the actual script to deploy the upgradable version of the MerlinLink smart contract.

The deployment process consists of the following steps:
1. Copy the config file `MerlinConfig.sol` to the `contracts/utils` folder and set up system invariants based on the selected network (testnet or mainnet).
2. Build MerlinLink smart contracts.
3. Read initialization parameters (supported tokens) from `@merlinlink/presets`.
4. Deploy the upgradable version of MerlinLink with the signer given by environment variables.
5. Write the address of the deployed contract back to `@merlinlink/presets`.

See the actual deploy scripts located in `scripts/deploy.js`. The connection config to different blockchains is provided by the `@merlinlink/presets` SDK in the `packages/presets/testnets.json` and `packages/presets/mainnets.json` files.

## Constructor Parameters
MerlinLink uses a whitelist for supported stablecoins (`address[] supportedTokens`), specified during the first deployment as a constructor parameter. The deploy script will read tokens from `@merlinlink/presets` and set them for each chain.

## Become a Liquidity Provider
Any user who wishes to become a MerlinLink liquidity provider must first register a pool index (by calling the contract method `depositAndRegister`) and transfer supported stablecoins into the MerlinLink contract. Related operations are provided in `scripts/pool.js`. Open the corresponding file, edit the parameters (including token symbol, deposit amount, pool index), and run `yarn hardhat pool --testnet [id]` or `yarn hardhat pool --mainnet [id]` to execute the deposit operation. The `pool.js` files also provide withdrawal scripts; please use them as needed.

