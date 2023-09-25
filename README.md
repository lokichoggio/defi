# DeFi

This project is meant to be a stablecoin where users can deposit WETH and WBTC in exchange for a token that will be pegged to the USD.

- [DeFi](#defi)
- [Getting Started](#getting-started)
  - [Requirements](#requirements)
  - [Quickstart](#quickstart)
    - [Optional Gitpod](#optional-gitpod)
- [Updates](#updates)
- [Usage](#usage)
  - [Start a local node](#start-a-local-node)
  - [Deploy](#deploy)
  - [Deploy - Other Network](#deploy---other-network)
  - [Testing](#testing)
    - [Test Coverage](#test-coverage)
- [Deployment to a testnet or mainnet](#deployment-to-a-testnet-or-mainnet)
  - [Scripts](#scripts)
  - [Estimate gas](#estimate-gas)
- [Formatting](#formatting)
- [Test on sepolia](#test-on-sepolia)

# Getting Started

## Requirements

- [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
  - You'll know you did it right if you can run `git --version` and you see a response like `git version x.x.x`
- [foundry](https://getfoundry.sh/)
  - You'll know you did it right if you can run `forge --version` and you see a response like `forge 0.2.0 (816e00b 2023-03-16T00:05:26.396218Z)`

## Quickstart

```
git clone git@github.com:lokichoggio/defi.git
cd defi
forge build
```

### Optional Gitpod

If you can't or don't want to run and install locally, you can work with this repo in Gitpod. If you do this, you can skip the `clone this repo` part.

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#github.com/PatrickAlphaC/foundry-smart-contract-lottery-f23)

# Updates
- The latest version of openzeppelin-contracts has changes in the ERC20Mock file. To follow along with the course, you need to install version 4.8.3 which can be done by ```forge install openzeppelin/openzeppelin-contracts@v4.8.3 --no-commit``` instead of ```forge install openzeppelin/openzeppelin-contracts --no-commit```

# Usage

## Start a local node

```
make anvil
```

## Deploy

This will default to your local node. You need to have it running in another terminal in order for it to deploy.

```
make deploy
```

## Deploy - Other Network

[See below](#deployment-to-a-testnet-or-mainnet)

## Testing

We talk about 4 test tiers in the video. 

1. Unit
2. Integration
3. Forked
4. Staging

In this repo we cover #1 and Fuzzing. 

```
forge test
```

### Test Coverage

```
forge coverage
```

and for coverage based testing: 

```
forge coverage --report debug
```


# Deployment to a testnet or mainnet

1. Setup environment variables

You'll want to set your `SEPOLIA_RPC_URL` and `SEPOLIA_PRIVATE_KEY` as environment variables. You can add them to a `.env` file, similar to what you see in `.env.example`.

- `SEPOLIA_PRIVATE_KEY`: The private key of your account (like from [metamask](https://metamask.io/)). **NOTE:** FOR DEVELOPMENT, PLEASE USE A KEY THAT DOESN'T HAVE ANY REAL FUNDS ASSOCIATED WITH IT.
  - You can [learn how to export it here](https://metamask.zendesk.com/hc/en-us/articles/360015289632-How-to-Export-an-Account-Private-Key).
- `SEPOLIA_RPC_URL`: This is url of the sepolia testnet node you're working with. You can get setup with one for free from [Alchemy](https://alchemy.com/?a=673c802981)

Optionally, add your `ETHERSCAN_API_KEY` if you want to verify your contract on [Etherscan](https://etherscan.io/).

1. Get testnet ETH

Head over to [faucets.chain.link](https://faucets.chain.link/) and get some testnet ETH. You should see the ETH show up in your metamask.

2. Deploy

```
make deploy ARGS="--network sepolia"
```

## Scripts

Instead of scripts, we can directly use the `cast` command to interact with the contract. 

For example, on Sepolia:

1. Get some WETH 

```
cast send 0xdd13E55209Fd76AfE204dBda4007C227904f0a81 "deposit()" --value 0.1ether --rpc-url $SEPOLIA_RPC_URL --private-key $SEPOLIA_PRIVATE_KEY
```

2. Approve the WETH

```
cast send 0xdd13E55209Fd76AfE204dBda4007C227904f0a81 "approve(address,uint256)" 0x091EA0838eBD5b7ddA2F2A641B068d6D59639b98 1000000000000000000 --rpc-url $SEPOLIA_RPC_URL --private-key $SEPOLIA_PRIVATE_KEY
```

3. Deposit and Mint DSC

```
cast send 0x091EA0838eBD5b7ddA2F2A641B068d6D59639b98 "depositCollateralAndMintDsc(address,uint256,uint256)" 0xdd13E55209Fd76AfE204dBda4007C227904f0a81 100000000000000000 10000000000000000 --rpc-url $SEPOLIA_RPC_URL --private-key $SEPOLIA_PRIVATE_KEY
```


## Estimate gas

You can estimate how much gas things cost by running:

```
forge snapshot
```

And you'll see an output file called `.gas-snapshot`


# Formatting


To run code formatting:
```
forge fmt
```

# Test on sepolia

```
forge inspect DecentralizedStableCoin methods         
{
  "allowance(address,address)": "dd62ed3e",
  "approve(address,uint256)": "095ea7b3",
  "balanceOf(address)": "70a08231",
  "burn(uint256)": "42966c68",
  "burnFrom(address,uint256)": "79cc6790",
  "decimals()": "313ce567",
  "decreaseAllowance(address,uint256)": "a457c2d7",
  "increaseAllowance(address,uint256)": "39509351",
  "mint(address,uint256)": "40c10f19",
  "name()": "06fdde03",
  "owner()": "8da5cb5b",
  "renounceOwnership()": "715018a6",
  "symbol()": "95d89b41",
  "totalSupply()": "18160ddd",
  "transfer(address,uint256)": "a9059cbb",
  "transferFrom(address,address,uint256)": "23b872dd",
  "transferOwnership(address)": "f2fde38b"
}

forge inspect DSCEngine methods
{
  "burnDsc(uint256)": "f6876608", # cast sig "burnDsc(uint256)"
  "calculateHealthFactor(uint256,uint256)": "01f72884",
  "depositCollateral(address,uint256)": "a5d5db0c",
  "depositCollateralAndMintDsc(address,uint256,uint256)": "e90db8a3",
  "getAccountCollateralValue(address)": "7d1a4450",
  "getAccountInformation(address)": "7be564fc",
  "getAdditionalFeedPrecision()": "8f63d667",
  "getCollateralBalanceOfUser(address,address)": "31e92b83",
  "getCollateralTokenPriceFeed(address)": "1c08adda",
  "getCollateralTokens()": "b58eb63f",
  "getCollatoralDeposited(address,address)": "2a102033",
  "getDsc()": "deb8e018",
  "getDscMinted(address)": "38e0f7cf",
  "getHealthFactor(address)": "fe6bcd7c",
  "getLiquidationBonus()": "59aa9e72",
  "getLiquidationPrecision()": "6c8102c0",
  "getLiquidationThreshold()": "4ae9b8bc",
  "getMinHealthFactor()": "8c1ae6c8",
  "getPrecision()": "9670c0bc",
  "getTokenAmountFromUsd(address,uint256)": "afea2e48",
  "getUsdValue(address,uint256)": "c660d112",
  "liquidate(address,address,uint256)": "26c01303",
  "mintDsc(uint256)": "c9b7c327",
  "redeemCollateral(address,uint256)": "9acd81b3",
  "redeemCollateralForDsc(address,uint256,uint256)": "f419ea9c"
}
```

```
make deploy ARGS="--network sepolia"

DecentralizedStableCoin 0x4Af59f5bfb4Ce7Aa98a8a3961c09eb09774ae3CF
DSCEngine 0x2B5Bb36F98B1f361b4FE00fD2b2cD4DA54b9C56E
HelperConfig 0xC7f2Cf4845C6db0e1a1e91ED41Bcd0FcC1b0E141
```

```
 cast call 0x4Af59f5bfb4Ce7Aa98a8a3961c09eb09774ae3CF "name()(string)" --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY
DecentralizedStableCoin

cast call 0x2B5Bb36F98B1f361b4FE00fD2b2cD4DA54b9C56E "getCollateralTokens()(address[])" --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY
[0xdd13E55209Fd76AfE204dBda4007C227904f0a81,0x8f3Cf7ad23Cd3CaDbD9735AFf958023239c6A063]


cast call 0xdd13E55209Fd76AfE204dBda4007C227904f0a81 "name()(string)" --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY
Wrapped Ether

cast send 0xdd13E55209Fd76AfE204dBda4007C227904f0a81 "deposit()" --value 0.1ether --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY

cast call 0xdd13E55209Fd76AfE204dBda4007C227904f0a81 "balanceOf(address)(uint256)" 0x8021f00c28C0e43788AE1a11f1F97eBf80d131bD --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY
100000000000000000

cast send 0xdd13E55209Fd76AfE204dBda4007C227904f0a81 "approve(address,uint256)" 0x2B5Bb36F98B1f361b4FE00fD2b2cD4DA54b9C56E 100000000000000000 --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY
txHash: 0xb67f674e0bfad224089540ca05cc659051f1fccc2fdccf490ea991f8f9a1da22

cast send 0x2B5Bb36F98B1f361b4FE00fD2b2cD4DA54b9C56E "depositCollateralAndMintDsc(address,uint256,uint256)" 0xdd13E55209Fd76AfE204dBda4007C227904f0a81 100000000000000000 100000000000000000 --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY
```
