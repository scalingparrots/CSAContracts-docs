# CSA Contracts
>
> Repository for CSA Smart Contracts on Polygon POS.

The CSA Contracts suite provides a set of contracts, enabling users to bet on the value of a football player. It operates on the Polygon POS and leverages Web3 Functions as oracles to update the values of footballers.

## Features

- **CSAT and CSAC Tokens**: CSAT is tradable and transferable, whereas CSAC is used for betting.
- **CSAConverter Contract**: Allows 1:1 conversion between CSAT and CSAC.
- **CSASwap Contract**: Enables users to swap CSA tokens for whitelisted tokens (USDC, USDT, and Matic).
- **CSAMarketplace Contract**: Facilitates buying and selling of CSAT tokens in a secondary market.
- **CSAOracle Contract**: Receives the data from a Web3 Function and updates the value of a footballer.
- **CSAEngine Contract**: Enables users to bet long or short on the value of a footballer.

## Deployed Contracts on Polygon POS

Below are the verified contracts deployed on Polygon POS.

| Contract Name     | Address                                                                                                    | Additional Information                                                                       |
|-------------------|------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------|
| CSAOracleV2       | [0x0ec57c17a94f67980672db9489b7f390fdd60645](https://polygonscan.com/address/0x0ec57c17a94f67980672db9489b7f390fdd60645) | Not upgradeable, redeployment needed if changes are required. [Documentation](src/CSAOracleV2.sol/contract.CSAOracleV2.html) |
| web3function      | [Link to Task](https://beta.app.gelato.network/task/0x4fbad2628c2b76da12ff912688fb9bfb6f486a221a0a25454a1c2af9c12cdff5?chainId=137) | [Beta App Gelato Network](https://beta.app.gelato.network)                                  |
| CSASwap           | [0xc91366ea832a33091fdcd0a8d00984f63689bcc1](https://polygonscan.com/address/0xc91366ea832a33091fdcd0a8d00984f63689bcc1) | [Implementation](https://polygonscan.com/address/0xd8881a4786acb8398081de719feb4190c57c9a4b) [Documentation](src/CSASwap.sol/contract.CSASwap.html)                            |
| CSAEngineV2       | [0x1b1c1a1be0f62d48e795aee0d39da08dba203859](https://polygonscan.com/address/0x1b1c1a1be0f62d48e795aee0d39da08dba203859) | [Implementation](https://polygonscan.com/address/0x5cec04b8817f60baa2613d06f2feaff83b717768) [Documentation](src/CSAEngineV2.sol/contract.CSAEngineV2.html)                    |
| CSAMarketplace    | [0xbb44ab1bd78ec31fa13cb85110125603291dc207](https://polygonscan.com/address/0xbb44ab1bd78ec31fa13cb85110125603291dc207) | [Implementation](https://polygonscan.com/address/0x9384d7602648307bf681ab64a0e7de48a765ee64) [Documentation](src/CSAMarketplace.sol/contract.CSAMarketplace.html)              |
| CSAT              | [0x6434fdd25b050150731213121e2ffcfbc071c6ef](https://polygonscan.com/address/0x6434fdd25b050150731213121e2ffcfbc071c6ef) | [Implementation](https://polygonscan.com/address/0xd5b037d90dec320e188de5b411040d0665aa3ab1) [Documentation](src/CSAT.sol/contract.CSAT.html)                                  |                            |               |
| CSAVault          | [0x12da0a2569f7d3cf1c903d2c76c13c7c2325446e](https://polygonscan.com/address/0x12da0a2569f7d3cf1c903d2c76c13c7c2325446e) | Not upgradeable for security reasons. [Documentation](src/CSAVault.sol/contract.CSAVault.html)                          |
| CSATBuyer         | [0x1750d1bb3064c80e0373508e4f06a9c8ef7efbab](https://polygonscan.com/address/0x1750d1bb3064c80e0373508e4f06a9c8ef7efbab) | [Implementation](https://polygonscan.com/address/0xf8b713c7513d16bf66d62686c58bd3b90943aee1) |
| USDC              | [0x3c499c542cef5e3811e1192ce70d8cc03d5c3359](https://polygonscan.com/token/0x3c499c542cef5e3811e1192ce70d8cc03d5c3359) |                                                                                              |
| TransferManager   | [0x3e76ded7924ec111c14b2eff2852fba4ce481f7e](https://polygonscan.com/address/0x3e76ded7924ec111c14b2eff2852fba4ce481f7e) |                                                                                              |


## What are Web3 Functions?

Web3 Functions are decentralized cloud functions that work similarly to AWS Lambda or Google Cloud, just for web3. They enable developers to execute on-chain transactions based on arbitrary off-chain data (APIs / subgraphs, etc) & computation. These functions are written in Typescript, stored on IPFS and run by Gelato.

## Documentation

You can find the official Web3 Functions documentation [here](https://docs.gelato.network/developer-services/web3-functions).

## Private Beta Restriction

Web3 Functions are currently in private Beta and can only be used by whitelisted users. If you would like to be added to the waitlist, please reach out to the team on [Discord](https://discord.com/invite/ApbA39BKyJ) or apply using this [form](https://form.typeform.com/to/RrEiARiI).

## Table of Content

- [What are Web3 Functions?](#what-are-web3-functions)
- [Documentation](#documentation)
- [Private Beta Restriction](#private-beta-restriction)
- [Project Setup Web3 Functions](#project-setup-web3-functions)
- [Project Setup Foundry](#project-setup-foundry)
  - [Setup](#setup)
  - [Compile](#compile)
  - [Test](#test)
  - [Format Code](#format-code)
  - [Coverage](#coverage)
  - [Setup coverage html](#setup-coverage-html)
  - [Contracts documentation](#contracts-documentation)
- [Test the CSA web3 function](#test-the-csa-web3-function)
  - [Calling your Web3 Function](#calling-your-web3-function)
- [Deploy CSA Web3Function on IPFS](#deploy-csa-web3function-on-ipfs)

## Project Setup Web3 Functions

1. Install project dependencies

```sh
yarn install
```

2. Configure your local environment:

- Copy `.env.example` to init your own `.env` file

```sh
cp .env.example .env
```

- Complete your `.env` file with your private settings

Make sure to set the `PRIVATE_KEY` to a whitelisted Gelato account as said in [Private Beta Restriction](#private-beta-restriction).

## Project Setup Foundry

You will need a copy of [Foundry](https://github.com/foundry-rs/foundry) installed before proceeding. See the [installation guide](https://github.com/foundry-rs/foundry#installation) for details.

### Setup

```sh
forge install
```

### Compile
  
```sh
forge build
```

### Test

```sh
forge test -vvv -f <POLYGON_POS_RPC_URL>
```

### Format code
  
```sh
forge fmt
```

### Coverage
  
```sh
forge coverage --ir-minimum -f <POLYGON_POS_RPC_URL>
```

#### Coverage with debug logs

```sh
forge coverage --ir-minimum -f <POLYGON_POS_RPC_URL> --report debug
```

### Setup coverage html

#### Install genhtml

```sh
brew install genhtml
```

#### Create a coverage directory in your foundry project
  
```sh
mkdir coverage
```

#### Run the following command

```sh
forge coverage --ir-minimum -f <POLYGON_POS_RPC_URL> --report lcov && genhtml lcov.info --branch-coverage --output-dir coverage
```

#### Open the index.html file in the coverage directory

`coverage/index.html`

#### Common error: No available formula with the name "genhtml". Did you mean ekhtml?

If you get this error, do:

```sh
brew install ekhtml
```

### Contracts documentation

#### Generate the documentation

```sh
forge doc --build
```

#### Serve the documentation

```sh
forge doc --serve --port 4000
```

## Test the CSA web3 function

### Calling your web3 function

- Use `npx w3f test src/web3-functions/CSA-oracle/index.ts` command to test the CSA oracle function

- Options:
  - `--logs` Show internal Web3 Function logs
  - `--debug` Show Runtime debug messages
  - `--chain-id=[number]` Specify the chainId to be used for your Web3 Function (default: `5` for Goerli)

## Deploy CSA Web3Function on IPFS

Use `npx w3f deploy src/web3-functions/CSA-oracle/index.ts` command to deploy your web3 function.
