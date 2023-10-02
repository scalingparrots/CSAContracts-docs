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
| CSAOracleV2       | [0x0ec57c17a94f67980672db9489b7f390fdd60645](https://polygonscan.com/address/0x0ec57c17a94f67980672db9489b7f390fdd60645) | [Documentation](src/CSAOracleV2.sol/contract.CSAOracleV2.html)                    |
| web3function      | [Link to Task](https://beta.app.gelato.network/task/0x4fbad2628c2b76da12ff912688fb9bfb6f486a221a0a25454a1c2af9c12cdff5?chainId=137) | [Beta App Gelato Network](https://beta.app.gelato.network)                                  |
| CSASwap           | [0x171b63cf580fb0016bffe7b87f0daa0ae9a015ef](https://polygonscan.com/address/0x171b63cf580fb0016bffe7b87f0daa0ae9a015ef) | [Documentation](src/CSASwap.sol/contract.CSASwap.html)                            |
| CSAEngineV2       | [0xfd193a7e3508de78bdebb78acb26a6033252bbb4](https://polygonscan.com/address/0xfd193a7e3508de78bdebb78acb26a6033252bbb4) | [Documentation](src/CSAEngineV2.sol/contract.CSAEngineV2.html)                    |
| CSAMarketplace    | [0x9f25ef1da3789037002a42670915f8f0b6e94f6e](https://polygonscan.com/address/0x9f25ef1da3789037002a42670915f8f0b6e94f6e) | [Documentation](src/CSAMarketplace.sol/contract.CSAMarketplace.html)              |
| CSAT              | [0x9b2fcc79907858117c841f5694b0c771836b292d](https://polygonscan.com/address/0x9b2fcc79907858117c841f5694b0c771836b292d) | [Documentation](src/CSAT.sol/contract.CSAT.html)                                  |
| CSAC              | [0x4bbd9c8b02d9a44cab01b7e7cd2ed75c1116c3e7](https://polygonscan.com/address/0x4bbd9c8b02d9a44cab01b7e7cd2ed75c1116c3e7) | [Documentation](src/CSAC.sol/contract.CSAC.html)                                  |
| CSAConverter      | [0xa32353be31238765c59c4a213a3a9487ec0eb96c](https://polygonscan.com/address/0xa32353be31238765c59c4a213a3a9487ec0eb96c) | [Documentation](src/CSAConverter.sol/contract.CSAConverter.html)                  |
| CSAVault          | [0x0267e6d97443541ec285edbe1d4c107a45a28e03](https://polygonscan.com/address/0x0267e6d97443541ec285edbe1d4c107a45a28e03) | [Documentation](src/CSAVault.sol/contract.CSAVault.html)                          |

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
