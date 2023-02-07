---
layout: ../../../../layouts/MainLayout.astro
section: vrf
date: Last Modified
title: "Supported Networks"
permalink: "docs/vrf/v2/direct-funding/supported-networks/"
metadata:
  title: "Chainlink VRF Contract Addresses"
  linkToWallet: true
  image:
    0: "/files/OpenGraph_V3.png"
setup: |
  import VrfCommon from "@features/vrf/v2/common/VrfCommon.astro"
  import ResourcesCallout from "@features/resources/callouts/ResourcesCallout.astro"
---

<VrfCommon callout="directFunding"/>

Chainlink VRF allows you to integrate provably fair and verifiably random data in your smart contract.

For implementation details, read [Introduction to Chainlink VRF v2 Direct funding method](/vrf/v2/direct-funding/).

## Wrapper Parameters

These parameters are configured in the VRF v2 Wrapper contract. You can view these values by running `getConfig` on the VRF v2 Wrapper or by viewing the VRF v2 Wrapper contract in a blockchain explorer.

- `uint32 stalenessSeconds`: How long the VRF v2 Wrapper waits until we consider the ETH/LINK price used for converting gas costs to LINK is stale and use `fallbackWeiPerUnitLink`.
- `uint32 wrapperGasOverhead`: The gas overhead of the VRF v2 Wrapper's `fulfillRandomWords` function.
- `uint32 coordinatorGasOverhead`: The gas overhead of the coordinator's `fulfillRandomWords` function.
- `uint8 maxNumWords`: Maximum number of words that can be requested in a single wrapped VRF request.

## Coordinator Parameters

Some parameters are important to know and are configured in the coordinator contract. You can view these values by running `getConfig` on the coordinator or by viewing the coordinator contract in a blockchain explorer.

- `uint16 minimumRequestConfirmations`: The minimum number of confirmation blocks on VRF requests before oracles respond
- `uint32 maxGasLimit`: The maximum gas limit supported for a `fulfillRandomWords` callback. Note that you still need to substract the `wrapperGasOverhead` for the accurate limit, as explained in [Direct funding limits](/vrf/v2/direct-funding/#limits).

## Fee Parameters

Fee parameters are configured in the VRF v2 Wrapper and the VRF v2 Coordinator contracts and specify the premium you pay per request in addition to the gas cost for the transaction. You can view them by running `getConfig` on the VRF v2 Wrapper:

- The `uint32 fulfillmentFlatFeeLinkPPM` parameter is a flat fee and defines the fees per request specified in millionths of LINK.
- The `uint8 wrapperPremiumPercentage` parameter defines the premium ratio in percentage. For example, a value of _0_ indicates no premium. A value of _15_ indicates a _15%_ premium.

The details for calculating the total transaction cost can be found [here](/vrf/v2/direct-funding/#request-and-receive-data).

## Configurations

<ResourcesCallout callout="bridgeRisks" />

- [Ethereum Mainnet](#ethereum-mainnet)
- [Goerli testnet](#goerli-testnet)
- [BNB Chain](#bnb-chain)
- [BNB Chain testnet](#bnb-chain-testnet)
- [Polygon Mainnet](#polygon-matic-mainnet)
- [Polygon Mumbai Testnet](#polygon-matic-mumbai-testnet)
- [Avalanche Mainnet](#avalanche-mainnet)
- [Avalanche Fuji Testnet](#avalanche-fuji-testnet)
- [Fantom Mainnet](#fantom-mainnet)
- [Fantom Testnet](#fantom-testnet)

### Ethereum Mainnet

| Item                       | Value                                                                                                                                                                                                        |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| LINK Token                 | <a class="erc-token-address" id="1_0x514910771AF9Ca656af840dff83E8264EcF986CA" href="https://etherscan.io/token/0x514910771AF9Ca656af840dff83E8264EcF986CA">`0x514910771AF9Ca656af840dff83E8264EcF986CA`</a> |
| VRF Wrapper                | [`0x5A861794B927983406fCE1D062e00b9368d97Df6`](https://etherscan.io/address/0x5A861794B927983406fCE1D062e00b9368d97Df6)                                                                                      |
| VRF Coordinator            | [`0x271682DEB8C4E0901D1a1550aD2e64D568E69909`](https://etherscan.io/address/0x271682DEB8C4E0901D1a1550aD2e64D568E69909)                                                                                      |
| Wrapper Premium Percentage | 0                                                                                                                                                                                                            |
| Coordinator Flat Fee       | 0.25 LINK                                                                                                                                                                                                    |
| Maximum Confirmations      | 200                                                                                                                                                                                                          |
| Maximum Random Values      | 10                                                                                                                                                                                                           |
| Wrapper Gas overhead       | 40000                                                                                                                                                                                                        |
| Coordinator Gas Overhead   | 90000                                                                                                                                                                                                        |

### Goerli testnet

:::note[Goerli Faucets]
Testnet LINK is available from https://faucets.chain.link/goerli<br/>
Testnet ETH is available from https://goerlifaucet.com/ or faucets listed at https://faucetlink.to/goerli
:::

| Item                       | Value                                                                                                                                                                                                               |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| LINK Token                 | <a class="erc-token-address" id="5_0x326C977E6efc84E512bB9C30f76E30c160eD06FB" href="https://goerli.etherscan.io/token/0x326C977E6efc84E512bB9C30f76E30c160eD06FB">`0x326C977E6efc84E512bB9C30f76E30c160eD06FB`</a> |
| VRF Wrapper                | [`0x708701a1DfF4f478de54383E49a627eD4852C816`](https://goerli.etherscan.io/address/0x708701a1DfF4f478de54383E49a627eD4852C816)                                                                                      |
| VRF Coordinator            | [`0x2ca8e0c643bde4c2e08ab1fa0da3401adad7734d`](https://goerli.etherscan.io/address/0x2ca8e0c643bde4c2e08ab1fa0da3401adad7734d)                                                                                      |
| Wrapper Premium Percentage | 0                                                                                                                                                                                                                   |
| Coordinator Flat Fee       | 0.25 LINK                                                                                                                                                                                                           |
| Maximum Confirmations      | 200                                                                                                                                                                                                                 |
| Maximum Random Values      | 10                                                                                                                                                                                                                  |
| Wrapper Gas overhead       | 40000                                                                                                                                                                                                               |
| Coordinator Gas Overhead   | 90000                                                                                                                                                                                                               |

### BNB Chain

:::tip[Important]
The LINK provided by the [BNB Chain Bridge](https://www.bnbchain.world/en/bridge) is not ERC-677 compatible, so cannot be used with Chainlink oracles. However, it can be [**converted to the official LINK token on BNB Chain using Chainlink's PegSwap service**](https://pegswap.chain.link/).
:::

| Item                       | Value                                                                                                                                                                                                        |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| LINK Token                 | <a class="erc-token-address" id="56_0x404460C6A5EdE2D891e8297795264fDe62ADBB75" href="https://bscscan.com/token/0x404460C6A5EdE2D891e8297795264fDe62ADBB75">`0x404460C6A5EdE2D891e8297795264fDe62ADBB75`</a> |
| VRF Wrapper                | [`0x721DFbc5Cfe53d32ab00A9bdFa605d3b8E1f3f42`](https://bscscan.com/address/0x721DFbc5Cfe53d32ab00A9bdFa605d3b8E1f3f42)                                                                                       |
| VRF Coordinator            | [`0xc587d9053cd1118f25F645F9E08BB98c9712A4EE`](https://bscscan.com/address/0xc587d9053cd1118f25F645F9E08BB98c9712A4EE)                                                                                       |
| Wrapper Premium Percentage | 0                                                                                                                                                                                                            |
| Coordinator Flat Fee       | 0.005 LINK                                                                                                                                                                                                   |
| Maximum Confirmations      | 200                                                                                                                                                                                                          |
| Maximum Random Values      | 10                                                                                                                                                                                                           |
| Wrapper Gas overhead       | 40000                                                                                                                                                                                                        |
| Coordinator Gas Overhead   | 90000                                                                                                                                                                                                        |

### BNB Chain testnet

:::note[BNB Chain Faucet]
Testnet LINK is available from https://faucets.chain.link/chapel
:::

| Item                       | Value                                                                                                                                                                                                                  |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| LINK Token                 | <a class="erc-token-address" id="97_0x84b9B910527Ad5C03A9Ca831909E21e236EA7b06" href="https://testnet.bscscan.com/address/0x84b9B910527Ad5C03A9Ca831909E21e236EA7b06">`0x84b9B910527Ad5C03A9Ca831909E21e236EA7b06`</a> |
| VRF Wrapper                | [`0x699d428ee890d55D56d5FC6e26290f3247A762bd`](https://testnet.bscscan.com/address/0x699d428ee890d55D56d5FC6e26290f3247A762bd)                                                                                         |
| VRF Coordinator            | [`0x6A2AAd07396B36Fe02a22b33cf443582f682c82f`](https://testnet.bscscan.com/address/0x6A2AAd07396B36Fe02a22b33cf443582f682c82f)                                                                                         |
| Wrapper Premium Percentage | 0                                                                                                                                                                                                                      |
| Coordinator Flat Fee       | 0.005 LINK                                                                                                                                                                                                             |
| Maximum Confirmations      | 200                                                                                                                                                                                                                    |
| Maximum Random Values      | 10                                                                                                                                                                                                                     |
| Wrapper Gas overhead       | 40000                                                                                                                                                                                                                  |
| Coordinator Gas Overhead   | 90000                                                                                                                                                                                                                  |

### Polygon (Matic) Mainnet

:::tip[Important]
The LINK provided by the [Polygon (Matic) Bridge](https://wallet.polygon.technology/bridge) is not ERC-677 compatible, so cannot be used with Chainlink oracles. However, it can be [**converted to the official LINK token on Polygon (Matic) using Chainlink's PegSwap service**](https://pegswap.chain.link/)
:::

| Item                       | Value                                                                                                                                                                                                               |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| LINK Token                 | <a class="erc-token-address" id="137_0xb0897686c545045aFc77CF20eC7A532E3120E0F1" href="https://polygonscan.com/address/0xb0897686c545045aFc77CF20eC7A532E3120E0F1">`0xb0897686c545045aFc77CF20eC7A532E3120E0F1`</a> |
| VRF Wrapper                | [`0x4e42f0adEB69203ef7AaA4B7c414e5b1331c14dc`](https://polygonscan.com/address/0x4e42f0adEB69203ef7AaA4B7c414e5b1331c14dc)                                                                                          |
| VRF Coordinator            | [`0xAE975071Be8F8eE67addBC1A82488F1C24858067`](https://polygonscan.com/address/0xAE975071Be8F8eE67addBC1A82488F1C24858067)                                                                                          |
| Wrapper Premium Percentage | 0                                                                                                                                                                                                                   |
| Coordinator Flat Fee       | 0.0005 LINK                                                                                                                                                                                                         |
| Maximum Confirmations      | 200                                                                                                                                                                                                                 |
| Maximum Random Values      | 10                                                                                                                                                                                                                  |
| Wrapper Gas overhead       | 40000                                                                                                                                                                                                               |
| Coordinator Gas Overhead   | 90000                                                                                                                                                                                                               |

### Polygon (Matic) Mumbai Testnet

:::note[Mumbai Faucet]
Testnet LINK and MATIC are available from [the Polygon faucet](https://faucet.polygon.technology/) and https://faucets.chain.link/mumbai.
:::

| Item                       | Value                                                                                                                                                                                                                         |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| LINK Token                 | <a class="erc-token-address" id="80001_0x326C977E6efc84E512bB9C30f76E30c160eD06FB" href="https://mumbai.polygonscan.com/address/0x326C977E6efc84E512bB9C30f76E30c160eD06FB">`0x326C977E6efc84E512bB9C30f76E30c160eD06FB `</a> |
| VRF Wrapper                | [`0x99aFAf084eBA697E584501b8Ed2c0B37Dd136693`](https://mumbai.polygonscan.com/address/0x99aFAf084eBA697E584501b8Ed2c0B37Dd136693)                                                                                             |
| VRF Coordinator            | [`0x7a1BaC17Ccc5b313516C5E16fb24f7659aA5ebed`](https://mumbai.polygonscan.com/address/0x7a1BaC17Ccc5b313516C5E16fb24f7659aA5ebed)                                                                                             |
| Wrapper Premium Percentage | 0                                                                                                                                                                                                                             |
| Coordinator Flat Fee       | 0.0005 LINK                                                                                                                                                                                                                   |
| Maximum Confirmations      | 200                                                                                                                                                                                                                           |
| Maximum Random Values      | 10                                                                                                                                                                                                                            |
| Wrapper Gas overhead       | 40000                                                                                                                                                                                                                         |
| Coordinator Gas Overhead   | 90000                                                                                                                                                                                                                         |

### Avalanche Mainnet

| Item                       | Value                                                                                                                                                                                                              |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| LINK Token                 | <a class="erc-token-address" id="43114_0x5947BB275c521040051D82396192181b413227A3" href="https://snowtrace.io/address/0x5947BB275c521040051D82396192181b413227A3">`0x5947BB275c521040051D82396192181b413227A3`</a> |
| VRF Wrapper                | [`0x721DFbc5Cfe53d32ab00A9bdFa605d3b8E1f3f42`](https://snowtrace.io/address/0x721DFbc5Cfe53d32ab00A9bdFa605d3b8E1f3f42)                                                                                            |
| VRF Coordinator            | [`0xd5D517aBE5cF79B7e95eC98dB0f0277788aFF634`](https://snowtrace.io/address/0xd5D517aBE5cF79B7e95eC98dB0f0277788aFF634)                                                                                            |
| Wrapper Premium Percentage | 0                                                                                                                                                                                                                  |
| Coordinator Flat Fee       | 0.005 LINK                                                                                                                                                                                                         |
| Maximum Confirmations      | 200                                                                                                                                                                                                                |
| Maximum Random Values      | 10                                                                                                                                                                                                                 |
| Wrapper Gas overhead       | 40000                                                                                                                                                                                                              |
| Coordinator Gas Overhead   | 90000                                                                                                                                                                                                              |

### Avalanche Fuji Testnet

:::note[Avax Fuji Faucet]
Testnet LINK is available from https://faucets.chain.link/fuji
:::

| Item                       | Value                                                                                                                                                                                                                      |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| LINK Token                 | <a class="erc-token-address" id="43113_0x0b9d5D9136855f6FEc3c0993feE6E9CE8a297846" href="https://testnet.snowtrace.io/address/0x0b9d5D9136855f6FEc3c0993feE6E9CE8a297846">`0x0b9d5D9136855f6FEc3c0993feE6E9CE8a297846`</a> |
| VRF Wrapper                | [`0x9345AC54dA4D0B5Cda8CB749d8ef37e5F02BBb21`](https://testnet.snowtrace.io/address/0x9345AC54dA4D0B5Cda8CB749d8ef37e5F02BBb21)                                                                                            |
| VRF Coordinator            | [`0x2eD832Ba664535e5886b75D64C46EB9a228C2610`](https://testnet.snowtrace.io/address/0x2eD832Ba664535e5886b75D64C46EB9a228C2610)                                                                                            |
| Wrapper Premium Percentage | 0                                                                                                                                                                                                                          |
| Coordinator Flat Fee       | 0.005 LINK                                                                                                                                                                                                                 |
| Maximum Confirmations      | 200                                                                                                                                                                                                                        |
| Maximum Random Values      | 10                                                                                                                                                                                                                         |
| Wrapper Gas overhead       | 40000                                                                                                                                                                                                                      |
| Coordinator Gas Overhead   | 90000                                                                                                                                                                                                                      |

### Fantom Mainnet

:::tip[Important]
You must use ERC-677 LINK on Fantom. ERC-20 LINK will not work with Chainlink services.<br/>
Use [bridge.multichain.org](https://bridge.multichain.org/#/router) to send LINK to the Fantom network and be sure to select LINK-ERC677 as the token you will receive on the Fantom mainnet.
:::

| Item                       | Value                                                                                                                                                                                                         |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| LINK Token                 | <a class="erc-token-address" id="250_0x6F43FF82CCA38001B6699a8AC47A2d0E66939407" href="https://ftmscan.com/token/0x6F43FF82CCA38001B6699a8AC47A2d0E66939407">`0x6F43FF82CCA38001B6699a8AC47A2d0E66939407`</a> |
| VRF Wrapper                | [`0xeDA5B00fB33B13c730D004Cf5D1aDa1ac191Ddc2`](https://ftmscan.com/address/0xeDA5B00fB33B13c730D004Cf5D1aDa1ac191Ddc2)                                                                                        |
| VRF Coordinator            | [`0xd5D517aBE5cF79B7e95eC98dB0f0277788aFF634`](https://ftmscan.com/address/0xd5d517abe5cf79b7e95ec98db0f0277788aff634)                                                                                        |
| Wrapper Premium Percentage | 0                                                                                                                                                                                                             |
| Coordinator Flat Fee       | 0.0005 LINK                                                                                                                                                                                                   |
| Maximum Confirmations      | 200                                                                                                                                                                                                           |
| Maximum Random Values      | 10                                                                                                                                                                                                            |
| Wrapper Gas overhead       | 40000                                                                                                                                                                                                         |
| Coordinator Gas Overhead   | 90000                                                                                                                                                                                                         |

### Fantom Testnet

:::note[Fantom Testnet Faucet]
Testnet LINK is available from https://faucets.chain.link/fantom-testnet
:::

| Item                       | Value                                                                                                                                                                                                                    |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| LINK Token                 | <a class="erc-token-address" id="4002_0xfaFedb041c0DD4fA2Dc0d87a6B0979Ee6FA7af5F" href="https://testnet.ftmscan.com/address/0xfaFedb041c0DD4fA2Dc0d87a6B0979Ee6FA7af5F">`0xfaFedb041c0DD4fA2Dc0d87a6B0979Ee6FA7af5F`</a> |
| VRF Wrapper                | [`0x38336BDaE79747a1d2c4e6C67BBF382244287ca6`](https://testnet.ftmscan.com/address/0x38336BDaE79747a1d2c4e6C67BBF382244287ca6)                                                                                           |
| VRF Coordinator            | [`0xbd13f08b8352A3635218ab9418E340c60d6Eb418`](https://testnet.ftmscan.com/address/0xbd13f08b8352a3635218ab9418e340c60d6eb418)                                                                                           |
| Wrapper Premium Percentage | 0                                                                                                                                                                                                                        |
| Coordinator Flat Fee       | 0.0005 LINK                                                                                                                                                                                                              |
| Maximum Confirmations      | 200                                                                                                                                                                                                                      |
| Maximum Random Values      | 10                                                                                                                                                                                                                       |
| Wrapper Gas overhead       | 40000                                                                                                                                                                                                                    |
| Coordinator Gas Overhead   | 90000                                                                                                                                                                                                                    |
