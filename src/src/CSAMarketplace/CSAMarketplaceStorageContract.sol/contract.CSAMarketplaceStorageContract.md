# CSAMarketplaceStorageContract
**Author:**
Maurizio Murru

*This contract is the storage definition of the CSAMarketplace contract.*


## State Variables
### EURUSD_DECIMALS
*Decimals for EUR/USD oracle*


```solidity
uint16 public immutable EURUSD_DECIMALS;
```


### TokenUSD_DECIMALS
*Decimals for Token/USD oracle*


```solidity
uint16 public immutable TokenUSD_DECIMALS;
```


### CSAT
*CSA token*


```solidity
IERC20 public immutable CSAT;
```


### TOKEN_OUT
*Token to be exchanged for*


```solidity
IERC20 public immutable TOKEN_OUT;
```


### MIN_BUY_AMOUNT
*Minimum buy amount to avoid meta tx exploitation*


```solidity
uint256 public immutable MIN_BUY_AMOUNT;
```


### DUST_AMOUNT
*Amount of CSAT left in a sell order to be considered dust and be removed*


```solidity
uint256 public immutable DUST_AMOUNT;
```


### EURUSD_ORACLE
*Oracle to get the price of EUR/USD*


```solidity
AggregatorV3Interface public immutable EURUSD_ORACLE;
```


### TOKENUSD_ORACLE
*Oracle to get the price of Token/USD*


```solidity
AggregatorV3Interface public immutable TOKENUSD_ORACLE;
```


### VAULT
*Vault to hold the tokens*


```solidity
CSAVault public immutable VAULT;
```


### MAX_FEE
*Fee percentage*

*Maximum fee percentage (100%)*


```solidity
uint256 public constant MAX_FEE = 10000;
```


### MAX_N_ORDERS
*Maximum number of orders to match at the head of the queue*


```solidity
uint256 public constant MAX_N_ORDERS = 4;
```


### UPGRADER_ROLE
*Role identifier for pausers.*


```solidity
bytes32 public constant UPGRADER_ROLE = keccak256("UPGRADER_ROLE");
```


### FEE_SETTER_ROLE
*Role identifier for fee setters.*


```solidity
bytes32 public constant FEE_SETTER_ROLE = keccak256("FEE_SETTER_ROLE");
```


### CSAMarketplaceStorageLocation
*Storage location for CSAMarketplaceStorage*


```solidity
bytes32 private constant CSAMarketplaceStorageLocation =
    keccak256(abi.encode(uint256(keccak256("CSA.storage.CSAMarketplace")) - 1)) & ~bytes32(uint256(0xff));
```


### CSAMarketplaceQueueStorageLocation
*Storage location for CSAMarketplaceQueueStorage*


```solidity
bytes32 private constant CSAMarketplaceQueueStorageLocation =
    keccak256(abi.encode(uint256(keccak256("CSA.storage.CSAMarketplaceQueue")) - 1)) & ~bytes32(uint256(0xff));
```


### CSAMarketplaceUseLimiterStorageLocation
*Storage location for CSAMarketplaceUseLimiterStorage*


```solidity
bytes32 private constant CSAMarketplaceUseLimiterStorageLocation =
    keccak256(abi.encode(uint256(keccak256("CSA.storage.CSAMarketplaceUseLimiter")) - 1)) & ~bytes32(uint256(0xff));
```


## Functions
### constructor

*Constructor for the CSAMarketplaceStorageContract*


```solidity
constructor(
    address _csat,
    address _tokenOut,
    AggregatorV3Interface _EURUSDOracle,
    AggregatorV3Interface _TokenUSDOracle,
    address _vault,
    uint256 _minBuyAmount,
    uint256 _dustAmount
);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_csat`|`address`|Address of the CSA token|
|`_tokenOut`|`address`|Address of the token to be exchanged for|
|`_EURUSDOracle`|`AggregatorV3Interface`|Address of the Oracle to get the price of EUR/USD|
|`_TokenUSDOracle`|`AggregatorV3Interface`|Address of the Oracle to get the price of Token/USD|
|`_vault`|`address`|Address of the vault to send swapped tokens|
|`_minBuyAmount`|`uint256`|Minimum buy amount to avoid meta tx exploitation|
|`_dustAmount`|`uint256`|Amount of CSAT left in a sell order to be considered dust and be removed.|


### _getCSAMarketplaceStorage

*This function is used to get the CSAMarketplaceStorage storage pointer.*


```solidity
function _getCSAMarketplaceStorage() internal pure returns (CSAMarketplaceStorage storage $);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`$`|`CSAMarketplaceStorage`|The storage pointer to the CSAMarketplaceStorage.|


### _getCSAMarketplaceQueueStorage

*This function is used to get the CSAMarketplaceQueueStorage storage pointer.*


```solidity
function _getCSAMarketplaceQueueStorage() internal pure returns (CSAMarketplaceQueueStorage storage $);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`$`|`CSAMarketplaceQueueStorage`|The storage pointer to the CSAMarketplaceQueueStorage.|


### _getCSAMarketplaceUseLimiterStorage

*This function is used to get the CSAMarketplaceUseLimiterStorage storage pointer.*


```solidity
function _getCSAMarketplaceUseLimiterStorage() internal pure returns (CSAMarketpalceUseLimiterStorage storage $);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`$`|`CSAMarketpalceUseLimiterStorage`|The storage pointer to the CSAMarketplaceUseLimiterStorage.|


### csat

*Gets the CSAT contract address*


```solidity
function csat() external view returns (IERC20);
```

### tokenOut

*Gets the token to be exchanged for*


```solidity
function tokenOut() external view returns (IERC20);
```

### EURUSDOracle

*Gets the EUR/USD oracle contract.*


```solidity
function EURUSDOracle() external view returns (AggregatorV3Interface);
```

### TokenUSDOracle

*Gets the Token/USD oracle contract.*


```solidity
function TokenUSDOracle() external view returns (AggregatorV3Interface);
```

### vault

*Gets the vault contract.*


```solidity
function vault() external view returns (CSAVault);
```

### priceFee

*Gets the Marketplace fee percentage.*


```solidity
function priceFee() external view returns (uint16);
```

## Structs
### CSAMarketplaceStorage
*Marketplace storage struct*


```solidity
struct CSAMarketplaceStorage {
    uint16 priceFee;
}
```

### CSAMarketplaceQueueStorage
*Marketplace queue storage struct*


```solidity
struct CSAMarketplaceQueueStorage {
    CSAQueueLib.Queue sellOrders;
}
```

### CSAMarketplaceUseLimiterStruct
*Marketplace use limiter storage struct*


```solidity
struct CSAMarketplaceUseLimiterStruct {
    uint40 firstUseTodayTimestamp;
    uint16 usesToday;
}
```

### CSAMarketpalceUseLimiterStorage
*Marketplace use limiter storage*


```solidity
struct CSAMarketpalceUseLimiterStorage {
    mapping(address => CSAMarketplaceUseLimiterStruct) useLimiter;
}
```

