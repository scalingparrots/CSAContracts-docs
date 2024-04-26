# CSASwapStorageContract
**Author:**
Maurizio Murru https://github.com/akerbabber

*This contract is the storage definition of the CSASwap contract.*


## State Variables
### csat
*Address for the CSASwap storage.*


```solidity
CSAT public immutable csat;
```


### EURUSDOracle
*Address for the EUR/USD Oracle.*


```solidity
AggregatorV3Interface public immutable EURUSDOracle;
```


### EURUSDOracleDecimals
*Number of decimals of the EUR/USD Oracle.*


```solidity
uint8 public immutable EURUSDOracleDecimals;
```


### vault
*Address for the vault.*


```solidity
address public immutable vault;
```


### CSAT_DECIMALS
*Number of decimals of the CSAT token.*


```solidity
uint8 public immutable CSAT_DECIMALS;
```


### MAX_FEE
*Maximum allowable fee. 100% = 10000.*


```solidity
uint256 public constant MAX_FEE = 10000;
```


### CSASwapStorageLocation
*CSASwapStorageLocation is a constant bytes32 variable that represents the storage location of the CSASwapStorage.*


```solidity
bytes32 private constant CSASwapStorageLocation =
    keccak256(abi.encode(uint256(keccak256("CSA.storage.CSASwap")) - 1)) & ~bytes32(uint256(0xff));
```


### UPGRADER_ROLE
*UPGRADER_ROLE is a constant bytes32 variable that represents the role of an upgrader,
an upgrader is enabled to upgrade the contract.*


```solidity
bytes32 public constant UPGRADER_ROLE = 0x189ab7a9244df0848122154315af71fe140f3db0fe014031783b0946b8c9d2e3;
```


### FEE_SETTER_ROLE
*FEE_SETTER_ROLE is a constant bytes32 variable that represents the role of a fee setter.*


```solidity
bytes32 public constant FEE_SETTER_ROLE = keccak256("FEE_SETTER_ROLE");
```


## Functions
### _getCSASwapStorage

*This function is used to get the CSASwapStorage storage pointer.*


```solidity
function _getCSASwapStorage() internal pure returns (CSASwapStorage storage $);
```

### constructor

*Constructor for the CSASwapStorageContract.*


```solidity
constructor(address _csat, address _EURUSDOracle, address _vault);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_csat`|`address`|Address of the CSAT token contract|
|`_EURUSDOracle`|`address`|Address of the EUR/USD Oracle contract|
|`_vault`|`address`|Address of the vault to send swapped tokens|


### tokenInfos

*Function to get the TokenData of a token as a tuple.*


```solidity
function tokenInfos(address _address) external view returns (address, bool, uint8);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_address`|`address`|Address of the token|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The Oracle address, the whitelisted flag and the number of decimals of the token.|
|`<none>`|`bool`||
|`<none>`|`uint8`||


### tokenInfosStruct

*Function to get the TokenData of a token as a struct.*


```solidity
function tokenInfosStruct(address _address) external view returns (TokenData memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_address`|`address`|Address of the token|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`TokenData`|The TokenData struct of the token.|


### fee

*Function to get the fee percentage.*


```solidity
function fee() external view returns (uint16);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint16`|The fee percentage.|


## Structs
### TokenData
*Struct to store the token data.
This is used to store the Oracle address, the number of decimals and the whitelisted flag of the token.*


```solidity
struct TokenData {
    address oracleAddress;
    bool isWhitelisted;
    uint8 decimals;
}
```

### CSASwapStorage
*Struct to store the CSASwap storage.
This is used to store the fee and the token data.
It is used to not create memory slot collisions with the CSASwap contract.*


```solidity
struct CSASwapStorage {
    uint16 fee;
    mapping(address => TokenData) tokenInfos;
}
```

