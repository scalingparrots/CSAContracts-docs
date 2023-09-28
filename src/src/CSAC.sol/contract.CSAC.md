# CSAC
**Inherits:**
AccessControl

**Author:**
Maurizio Murru https://github.com/akerbabber

*This contract enables swapping of tokens and maintaining supply,
enforcing access controls for minting and burning functionalities.*

*Note: This contract does NOT adhere to the ERC20 standard and the
tokens are non-transferable.*


## State Variables
### _amounts
*Mapping of the amounts of tokens owned by each address.*


```solidity
mapping(address => uint256) private _amounts;
```


### _totalSupply
*Total number of tokens in existence.*


```solidity
uint256 private _totalSupply;
```


### CSAT
The address of the CSAT token contract


```solidity
address public immutable CSAT;
```


### ENABLED_CONTRACT_ROLE
Role identifier for enabling contract role


```solidity
bytes32 public constant ENABLED_CONTRACT_ROLE = keccak256("ENABLED_CONTRACT_ROLE");
```


## Functions
### constructor

Creates a new instance of the CSAC contract

*Assigns the sender the default admin role*


```solidity
constructor(address _CSAT);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_CSAT`|`address`|The address of the CSAT token contract|


### decimals

Returns the number of decimals the token uses


```solidity
function decimals() public view virtual returns (uint8);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|The number of decimals|


### totalSupply

Returns the total supply of tokens


```solidity
function totalSupply() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total supply|


### amountOf

Gets the amount of tokens owned by `account`.


```solidity
function amountOf(address account) public view virtual returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to query.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The amount of tokens owned by `account`.|


### swapFromCSAT

Swaps `amount` CSAT tokens to this contract's token

*Mints tokens to the sender and burns the same amount from sender's CSAT balance*


```solidity
function swapFromCSAT(uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of tokens to swap|


### swapToCSAT

Swaps `amount` of this contract's tokens to CSAT tokens

*Burns tokens from the sender and mints the same amount to sender's CSAT balance*


```solidity
function swapToCSAT(uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of tokens to swap|


### _mint

*Mints `amount` tokens to `account`, increasing total supply.*


```solidity
function _mint(address account, uint256 amount) internal virtual;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to mint tokens to.|
|`amount`|`uint256`|The amount of tokens to mint.|


### _burn

*Burns `amount` tokens from `account`, decreasing total supply.*


```solidity
function _burn(address account, uint256 amount) internal virtual;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to burn tokens from.|
|`amount`|`uint256`|The amount of tokens to burn.|


### mint

Mints `amount` tokens to `to` address

*Can only be called by addresses with the `ENABLED_CONTRACT_ROLE` role*


```solidity
function mint(address to, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|The address to mint tokens to.|
|`amount`|`uint256`|The amount of tokens to mint.|


### burn

Burns `amount` tokens from `from` address

*Can only be called by addresses with the `ENABLED_CONTRACT_ROLE` role*


```solidity
function burn(address from, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The address to burn tokens from.|
|`amount`|`uint256`|The amount of tokens to burn.|


## Errors
### NoEnabledContractRole
*Throws if called by any account other than one with the enabled contract role*


```solidity
error NoEnabledContractRole(address from);
```

### MintToZeroAddress
*Throws if trying to mint to the zero address*


```solidity
error MintToZeroAddress();
```

