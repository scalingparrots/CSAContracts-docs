# CSASwap
**Inherits:**
AccessControl, Pausable

**Author:**
Maurizio Murru https://github.com/akerbabber

This contract is used to swap various tokens and the native token for CSAT tokens.
It uses Chainlink oracles to get the prices of tokens and supports pausing functionality.


## State Variables
### csat

```solidity
CSAT public csat;
```


### tokenInfos

```solidity
mapping(address => TokenData) public tokenInfos;
```


### EURUSDOracle

```solidity
AggregatorV3Interface public EURUSDOracle;
```


### EURUSDOracleDecimals

```solidity
uint8 public EURUSDOracleDecimals;
```


### fee

```solidity
uint16 public fee;
```


### MAX_FEE

```solidity
uint256 public constant MAX_FEE = 10000;
```


### vault

```solidity
address payable public vault;
```


### nativeTokenUSDOracle

```solidity
address public nativeTokenUSDOracle;
```


### nativeTokenUSDOracleDecimals

```solidity
uint8 public nativeTokenUSDOracleDecimals;
```


### CSAT_DECIMALS

```solidity
uint256 public immutable CSAT_DECIMALS;
```


## Functions
### constructor

Constructs the CSASwap contract.


```solidity
constructor(address _csat, address _EURUSDOracle, address payable _vault, uint16 _fee, address _nativeTokenUSDOracle);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_csat`|`address`|Address of the CSAT token contract|
|`_EURUSDOracle`|`address`|Address of the EUR/USD Oracle contract|
|`_vault`|`address payable`|Address of the vault to send swapped tokens|
|`_fee`|`uint16`|Initial fee percentage|
|`_nativeTokenUSDOracle`|`address`|Address of the native token USD Oracle contract|


### addWhitelistedToken

Adds a token to the whitelist.


```solidity
function addWhitelistedToken(address _tokenAddress, address _oracleAddress) external onlyRole(DEFAULT_ADMIN_ROLE);
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


### changeEURUSDOracle

Changes the EUR/USD Oracle.


```solidity
function changeEURUSDOracle(address _EURUSDOracle) external onlyRole(DEFAULT_ADMIN_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_EURUSDOracle`|`address`|Address of the new EUR/USD Oracle|


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


### getNativeTokenPrice

Gets the latest price of the native token in USD.


```solidity
function getNativeTokenPrice() public view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The latest price of the native token in USD|


### swap

Swaps a specified amount of a token for CSAT tokens.


```solidity
function swap(address _tokenAddress, uint256 _amount) external whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_tokenAddress`|`address`|Address of the token to be swapped|
|`_amount`|`uint256`|Amount of the token to be swapped|


### swapNativeToken

Swaps a specified amount of the native token for CSAT tokens.

*The native token is the token that is used to pay for gas fees.*

*The native token is assumed to have 18 decimals.*

*The native token is assumed to be whitelisted.*

*Being a payable function, the amount received is stored in msg.value.*


```solidity
function swapNativeToken() external payable whenNotPaused;
```

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
event Swap(address indexed _from, address indexed _tokenAddress, uint256 _amount);
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

### NotWhitelisted

```solidity
error NotWhitelisted(address _address);
```

### InvalidFee

```solidity
error InvalidFee(uint16 _fee);
```

## Structs
### TokenData

```solidity
struct TokenData {
    address oracleAddress;
    bool isWhitelisted;
    uint8 decimals;
}
```

