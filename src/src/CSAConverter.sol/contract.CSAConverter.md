# CSAConverter
**Author:**
Maurizio Murru https://github.com/akerbabber

*This contract is designed to convert between CSAT and CSAC tokens.
It emits events for each conversion and utilizes interfaces for burning and minting tokens.*


## State Variables
### CSAT
The address of the CSAT token contract


```solidity
address public immutable CSAT;
```


### CSAC
The address of the CSAC token contract


```solidity
address public immutable CSAC;
```


## Functions
### constructor

Initializes the contract with addresses of CSAT and CSAC contracts


```solidity
constructor(address _CSAT, address _CSAC);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_CSAT`|`address`|The address of the CSAT token contract|
|`_CSAC`|`address`|The address of the CSAC token contract|


### csatToCsac

Converts `amount` CSAT tokens to CSAC tokens.

*Emits a {CsatToCsac} event.*


```solidity
function csatToCsac(uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of CSAT tokens to convert|


### csacToCsat

Converts `amount` CSAC tokens to CSAT tokens.

*Emits a {CsacToCsat} event.*


```solidity
function csacToCsat(uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of CSAC tokens to convert|


## Events
### CsatToCsac
*Emitted when CSAT tokens are converted to CSAC*


```solidity
event CsatToCsac(address indexed from, uint256 amount);
```

### CsacToCsat
*Emitted when CSAC tokens are converted to CSAT*


```solidity
event CsacToCsat(address indexed from, uint256 amount);
```

