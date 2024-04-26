# CSASwap
**Inherits:**
Initializable, AccessControlUpgradeable, PausableUpgradeable, UUPSUpgradeable, ERC2771ContextUpgradeable, [CSASwapStorageContract](/src/CSASwap/CSASwapStorageContract.sol/contract.CSASwapStorageContract.md)

**Author:**
Maurizio Murru https://github.com/akerbabber

This contract is used to swap various tokens and the native token for CSAT tokens.
It uses Chainlink oracles to get the prices of tokens and supports pausing functionality.


## Functions
### constructor

Constructor for the CSASwap contract.

*All variables are immutable, as such their values are hardcoded into the contract code*

*Changing them requires an upgrade of the contract*


```solidity
constructor(address _trustedForwarder, address _csat, address _EURUSDOracle, address _vault)
    CSASwapStorageContract(_csat, _EURUSDOracle, _vault)
    ERC2771ContextUpgradeable(_trustedForwarder);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_trustedForwarder`|`address`||
|`_csat`|`address`|Address of the CSAT token contract|
|`_EURUSDOracle`|`address`|Address of the EUR/USD Oracle contract|
|`_vault`|`address`|Address of the vault to send swapped tokens|


### initialize

Initializes the CSASwap contract.


```solidity
function initialize(uint16 _fee) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_fee`|`uint16`|Initial fee percentage|


### _authorizeUpgrade

*Reverts if called by any account other than an upgrader.*


```solidity
function _authorizeUpgrade(address newImplementation) internal override onlyRole(UPGRADER_ROLE);
```

### addWhitelistedToken

Adds a token to the whitelist.


```solidity
function addWhitelistedToken(address _tokenAddress, address _oracleAddress) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_tokenAddress`|`address`|Address of the token to be whitelisted|
|`_oracleAddress`|`address`|Address of the Oracle to get the price of the token in USD|


### removeWhitelistedToken

Removes a token from the whitelist.


```solidity
function removeWhitelistedToken(address _tokenAddress) external onlyRole(DEFAULT_ADMIN_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_tokenAddress`|`address`|Address of the token to be removed from the whitelist|


### getEURUSDPrice

Gets the latest price of EUR in USD from the EURUSD Oracle.


```solidity
function getEURUSDPrice() public view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The latest price of EUR in USD|


### getTokenPrice

Gets the latest price of the specified token in USD.


```solidity
function getTokenPrice(address _tokenAddress) public view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_tokenAddress`|`address`|Address of the token|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The latest price of the token in USD|


### _swap

Swaps a specified amount of a token for CSAT tokens.


```solidity
function _swap(address _tokenAddress, uint256 _amount, address receiver) internal whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_tokenAddress`|`address`|Address of the token to be swapped|
|`_amount`|`uint256`|Amount of the token to be swapped|
|`receiver`|`address`||


### swap

Swaps a specified amount of the native token for CSAT tokens.


```solidity
function swap(address _tokenAddress, uint256 _amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_tokenAddress`|`address`|Address of the whitelisted token to be swapped|
|`_amount`|`uint256`|Amount of the native token to be swapped|


### swapFor

Swaps a specified amount of the native token for CSAT tokens.


```solidity
function swapFor(address _tokenAddress, uint256 _amount, address receiver) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_tokenAddress`|`address`|Address of the whitelisted token to be swapped|
|`_amount`|`uint256`|Amount of the native token to be swapped|
|`receiver`|`address`|Address of the receiver of the CSAT tokens|


### pause

Pauses the contract, preventing token swaps.


```solidity
function pause() public whenNotPaused;
```

### unpause

Unpauses the contract, allowing token swaps.


```solidity
function unpause() public whenPaused;
```

### setFee

Sets the fee for the swaps.


```solidity
function setFee(uint16 _fee) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_fee`|`uint16`|The new fee percentage|


### _msgSender


```solidity
function _msgSender() internal view override(ContextUpgradeable, ERC2771ContextUpgradeable) returns (address sender);
```

### _msgData


```solidity
function _msgData() internal view override(ContextUpgradeable, ERC2771ContextUpgradeable) returns (bytes calldata);
```

### _contextSuffixLength


```solidity
function _contextSuffixLength()
    internal
    view
    override(ContextUpgradeable, ERC2771ContextUpgradeable)
    returns (uint256);
```

## Events
### AddToWhitelist

```solidity
event AddToWhitelist(address indexed _address);
```

### RemoveFromWhitelist

```solidity
event RemoveFromWhitelist(address indexed _address);
```

### ChangedEURUSDOracle

```solidity
event ChangedEURUSDOracle(address indexed _address);
```

### Swap

```solidity
event Swap(
    address indexed _from, address indexed _tokenAddress, uint256 _amount, address indexed _to, uint256 _csatAmount
);
```

### SwapNativeToken

```solidity
event SwapNativeToken(address indexed _from, uint256 _amount);
```

### ChangedFee

```solidity
event ChangedFee(uint16 _fee);
```

## Errors
### NotAdmin

```solidity
error NotAdmin(address _address);
```

### NotFeeSetter

```solidity
error NotFeeSetter(address _address);
```

### NotWhitelisted

```solidity
error NotWhitelisted(address _address);
```

### InvalidFee

```solidity
error InvalidFee(uint16 _fee);
```

