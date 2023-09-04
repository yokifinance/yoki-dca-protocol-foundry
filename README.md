# Testnet and local usage

1. Setup your wallet and infura rpc inside `.env` file. Make sure you are using correct network since it will be cloned.

   [https://app.infura.io/dashboard](https://app.infura.io/dashboard)

2. Start docker
   `docker-compose up`

_optional_: In order to refetch latest chain state or fix any issues you can simply rebuild the container `docker-compose up --build`

It will start local testnet RPC on `http://localhost:8545` and deploy contracts to it. Provided .env mnemonic and address will be the owner of contracts as well as given 1000ETH for playtesting. _Don't forget to add and switch to local network in your metamask_.

Testnet provides fast minting (transactions are executed immediately instead of 10-15s). It is also a complete fork of existing network which means that you can test with real assets without affecting real chain state.

# Deploy contracts

## Deploy

Rename `.env.example` to `.env` and setup env variables

Run

Production:

`polygon`

```
export $(grep -v '^#' .env | xargs)
forge script --rpc-url $RPC --chain-id $CHAIN_ID script/networks/All_Deploy_Polygon.s.sol --broadcast --legacy --verify
```

`optimism`

```
export $(grep -v '^#' .env | xargs)
forge script --rpc-url $RPC --chain-id $CHAIN_ID script/networks/All_Deploy_Optimism.s.sol --broadcast --legacy --verify
```

`bsc`

```
export $(grep -v '^#' .env | xargs)
forge script --rpc-url $RPC --chain-id $CHAIN_ID script/networks/All_Deploy_Binance.s.sol --broadcast --legacy --verify
```

`note: deploy goes for single chain only. IF you need to deploy on multiple chains - dont forget to update "RPC" and "CHAIN_ID" values in env file before rerunning both commands`
`tip: to save gas - you may want to deploy factory only and keep dcaImp and whitelist from first/old ones. In that case - use deployDCAV3Factory instead`
Local testnet:

```
forge script --rpc-url http://localhost:8545 script/All_Deploy.sol --broadcast --legacy
```

You can check deployed addresses in console or in /broadcast folder

## Verify

Setup apikeys and urls in `foundry.toml` and constructor args in txt file (see example in `factory-constructor-args.txt`)

Verify target contract using following command:

```
forge verify-contract --chain polygon 0x7a5B8E6c19ceA36Abc3b8f2C13962344207feA6b --watch --constructor-args-path factory-constructor-args.txt src/factories/DCAV3Factory.sol:DCAV3Factory
```

# Developing contracts

## Prerequisites

Forge and other foundry tools are required.
Foundry prerequisites can be installed using guide from here: https://book.getfoundry.sh/getting-started/installation

## Local testing

To run tests simply start
cmd: `forge test -vv`
to run specific test use test name from any test file:
ex: `forge test -vv --match-test test_retrieveFunds`

## Local network for tests

You can start local testnet via docker or cmd. Instruction below is how to setup working environment manually:

1. We need testnet RPC with a copy of mainnet.
   We will be using Infra for that cause: (Infra website)[https://app.infura.io/]

Make sure to enable Polygon network in your app settings (_in order to use polygon you will have to attach a credit card_, however it is free of charge)

2. Start local testnet as a fork
   `anvil -m "*your wallet mnemonic*" --fork-url https://polygon-mainnet.infura.io/*your infra api key here*`

We provide our private key so we can ahve access to USDT's on local chain for easier testing moving further. Alternatively - you can create a custom pool and distribute test tokens, but this scenario is up to you and moving forward it is assumed that you are using account with USDT's on mainnet.

You can test that you did everything correctly by running:
`cast interface 0xE592427A0AEce92De3Edee1F18E0157C05861564` - it should output the ABI of UniSwap

Don't forget to check USDT balance of your wallet
`cast call 0xc2132d05d31c914a87c6611c10748aeb04b58e8f "balanceOf(address)(uint256)" \*your wallet address\*`

## Deploy contract to testnet

Use scripts from `/script` folder

## Now we can test locally deployed contract with metamask / FE app / cast / hardhat / etc.

# Deployed contracts

## Factory

| chain    | env   | address                                    |
| -------- | ----- | ------------------------------------------ |
| Polygon  | dev   | 0xD966F00350352770F9A087A755F2Fb46a379B67f |
| Polygon  | stage | 0xb98D003F017A452A33cDbAade4ED5Cb4B8EBA81c |
| Polygon  | prod  | 0x93dC15813de39052CA6b0fde40c9D073339b29C7 |
|          |
| Optimism | dev   | 0xC952e7E894bC6D9E217F483611Dd58142419618E |
| Optimism | stage | 0x41b42a0cc89b9AA554e8fEEA5EFf893dD6eCD294 |
| Optimism | prod  | 0x8a03Bf5a5d2Aeed434Caaa30db467073A551354f |
|          |
| Binance  | dev   | 0x02c63197d2f93054398a706867aE0CAaf33002b2 |
| Binance  | stage | 0x                                         |
| Binance  | prod  | 0x                                         |
|          |
| opBNBtn  | dev   | 0xa65427d745Fa64d665C74688ed05793f07f1A037 |
| opBNBtn  | stage | 0x                                         |
| opBNBtn  | prod  | 0x                                         |

## Whitelist

| chain    | env   | address                                    |
| -------- | ----- | ------------------------------------------ |
| Polygon  | dev   | 0xa1c3DDCF368691ceFE6266051aF6601dF058c827 |
| Polygon  | stage | 0x9338c26583509E0af29761101810fa5469910FE0 |
| Polygon  | prod  | 0xfA248cCac23De39EC0c06AF4540Ee4b21f5814b1 |
|          |
| Optimism | dev   | 0xb1340E58954513b432875C0939D795bB01e3b907 |
| Optimism | stage | 0xb1340E58954513b432875C0939D795bB01e3b907 |
| Optimism | prod  | 0x661e63c5fAc61aB0A8a3489CC814C021C2C4fde2 |
|          |
| Binance  | dev   | 0x1B8EE524844DE43827F13007C3360024D7d09191 |
| Binance  | stage | 0x                                         |
| Binance  | prod  | 0x                                         |
|          |
| opBNBtn  | dev   | 0x085DdF516F7d04eb8a09a204Eb40966435d62F83 |
| opBNBtn  | stage | 0x                                         |
| opBNBtn  | prod  | 0x                                         |

## Dca implementation

| chain    | env   | address                                    |
| -------- | ----- | ------------------------------------------ |
| Polygon  | dev   | 0xE8B720e4C37eAc0b512DA062b8317d9dC3c3E3F0 |
| Polygon  | stage | 0x1fE25798d205D77CC8c9F0c7D4667F69aa93DDbe |
| Polygon  | prod  | 0x14dD93713869034aF6fbFa474aaC22c52c089B47 |
|          |
| Optimism | dev   | 0x86807bF5FaCEE7456882ffA1476b11657A135160 |
| Optimism | stage | 0x86807bF5FaCEE7456882ffA1476b11657A135160 |
| Optimism | prod  | 0x81859d17BB5B4D327951330237f7533011948059 |
|          |
| Binance  | dev   | 0xF8d3De6F50d9C3392e540A922FCD0cA3a69e9a80 |
| Binance  | stage | 0x                                         |
| Binance  | prod  | 0x                                         |
|          |
| opBNBtn  | dev   | 0xe3b23Ed609B84354B1d5031A7677be4Ae9517efA |
| opBNBtn  | stage | 0x                                         |
| opBNBtn  | prod  | 0x                                         |

# Wallets

TRASURY:
`dev`: `0x400d0dbd2240c8cF16Ee74E628a6582a42bb4f35`
`stage`: `0x400d0dbd2240c8cF16Ee74E628a6582a42bb4f35`
`prod`: `0x31F5c1B1fF78AF6FB721cD1376f1B7D69929A794` - both treasury and executor

EXECUTOR:
`dev`: `0x79dAe73Ec88a11FA4B9381Fe92865a1EAE5f3125`
`stage`: `0x79dAe73Ec88a11FA4B9381Fe92865a1EAE5f3125`
`prod`: `0x31F5c1B1fF78AF6FB721cD1376f1B7D69929A794`
