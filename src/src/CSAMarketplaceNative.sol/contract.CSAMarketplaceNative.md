# CSAMarketplaceNative
**Inherits:**
AccessControl

**Author:**
Maurizio Murru https://github.com/akerbabber

*This contract implements a marketplace for buying and selling CSAT tokens.
CSAT can be exchanged with the blockchain native token (ETH, MATIC, etc.).
It uses a queue data structure to match sell orders with buy orders.
The price of the token is determined by two Chainlink oracles: EUR/USD and Token/USD.
The contract also has a vault to store the tokens and a fee system for the marketplace.*


## State Variables
### csat
*CSA token*


```solidity
IERC20 public csat;
```


### tokenOut
*Token to be exchanged for*


```solidity
IERC20 public tokenOut;
```


### EURUSDOracle
*Oracle to get the price of EUR/USD*


```solidity
AggregatorV3Interface public EURUSDOracle;
```


### TokenUSDOracle
*Oracle to get the price of Token/USD*


```solidity
AggregatorV3Interface public TokenUSDOracle;
```


### vault
*Vault to hold the tokens*


```solidity
CSAVault public vault;
```


### priceFee
*Fee percentage*


```solidity
uint16 public priceFee;
```


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


### sellOrders
*Queue of sell orders*


```solidity
CSAQueueLib.Queue public sellOrders;
```


### MAX_FEE
*Maximum fee percentage (100%)*


```solidity
uint256 public constant MAX_FEE = 10000;
```


### MAX_N_ORDERS
*Maximum number of orders to match at the head of the queue*


```solidity
uint256 public constant MAX_N_ORDERS = 4;
```


## Functions
### constructor

*Constructor function for the CSAMarketplaceNative contract.*


```solidity
constructor(address _csat, address _EURUSDOracle, address _TokenUSDOracle, address _vault, uint16 _priceFee);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_csat`|`address`|The address of the CSAT token contract.|
|`_EURUSDOracle`|`address`|The address of the EUR/USD price oracle contract.|
|`_TokenUSDOracle`|`address`|The address of the token/USD price oracle contract.|
|`_vault`|`address`|The address of the CSAVaultNative contract.|
|`_priceFee`|`uint16`|The fee percentage charged on each transaction.|


### changeEURUSDOracle

*Changes the address of the EUR/USD price oracle contract.*


```solidity
function changeEURUSDOracle(address _EURUSDOracle) public onlyRole(DEFAULT_ADMIN_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_EURUSDOracle`|`address`|The new address of the EUR/USD price oracle contract.|


### changeTokenUSDOracle

*Changes the address of the token/USD price oracle contract.*


```solidity
function changeTokenUSDOracle(address _TokenUSDOracle) public onlyRole(DEFAULT_ADMIN_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_TokenUSDOracle`|`address`|The new address of the token/USD price oracle contract.|


### setVault

*Sets the address of the vault contract.*


```solidity
function setVault(address _vault) public onlyRole(DEFAULT_ADMIN_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_vault`|`address`|The address of the vault contract.|


### makeCsatSellOrder

*Creates a sell order for the specified amount of CSA tokens.*


```solidity
function makeCsatSellOrder(uint256 _amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_amount`|`uint256`|The amount of CSA tokens to sell.|


### cancelCsatSellOrder

*Cancels a sell order for CSA tokens in the marketplace.*


```solidity
function cancelCsatSellOrder(uint48 _orderId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_orderId`|`uint48`|The ID of the sell order to cancel. Emits a {NotOrderOwner} error if the caller is not the owner of the order. Transfers the amount of CSA tokens back to the order owner.|


### buy

*This function allows a user to buy CSAT from the marketplace by matching the user's buy order with the first sell order in the queue.
If the first sell order is not enough, it will look up for the second and repeat up until the fourth.
If up until the fourth is not enough, a fallback id can be supplied to finish matching the order.*


```solidity
function buy(uint48 _fallbackId) external payable;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_fallbackId`|`uint48`|The fallback id to use if the first four sell orders are not enough to match the buy order.|


### convertAmountsToTokenIn

*This function converts an array of base amounts to a proportional array of amounts in a given token.
The conversion is based on the proportion of each amount in the input array to the sum of all amounts
in the input array, scaled by the given `amountIn`.*


```solidity
function convertAmountsToTokenIn(uint256[] memory _amounts, uint256 amountIn) public pure returns (uint256[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_amounts`|`uint256[]`|An array of uint256 amounts representing the base amounts to be converted. Each element in this array will be converted to a corresponding amount in the token, based on its proportion to the sum of all elements in the array.|
|`amountIn`|`uint256`|The uint256 amount in the token to be distributed among the `_amounts`. This amount is used to scale the proportionally distributed amounts in the resulting array.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256[]`|amountsIn An array of uint256 representing the converted amounts in the token. Each element in this array is the converted amount of the corresponding element in the input `_amounts`, scaled by the `amountIn`.|


### convertAmountToEURPrice

*Converts the given amount of tokens to its equivalent EUR price.*


```solidity
function convertAmountToEURPrice(uint256 _amount) public view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_amount`|`uint256`|The amount of tokens to convert.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The equivalent EUR price of the given amount of tokens. Requirements: - The token and EUR/USD oracles must have a positive price.|


### convertAmountInCsatToTokenPrice

This function uses the latest round data from the TokenUSDOracle and EURUSDOracle to calculate the token price.

The token and CSAT decimals are obtained from their respective IERC20Metadata interfaces.

If the TokenUSDPrice or EURUSDPrice is less than zero, a PriceLessThanZero error is thrown.

*Converts an amount in CSAT to its equivalent token price.*


```solidity
function convertAmountInCsatToTokenPrice(uint256 _amount) public view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_amount`|`uint256`|The amount in CSAT to convert.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The equivalent token price in the token's decimals.|


### _getFallbackIdOrder

This function is expensive and should not be called on-chain.

The number of orders checked on top of the queue should not be included as fallbacks.

If no order is enough to be a fallback, it will revert.

*Returns the fallback order ID for a given amount of tokens,
it starts from the head of the queue, skips the first MAX_N_ORDERS orders
and goes down until it finds an order that is enough.*


```solidity
function _getFallbackIdOrder(uint256 _amount) internal view returns (uint48 currentId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_amount`|`uint256`|The amount of tokens to get the fallback order for.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`currentId`|`uint48`|The ID of the fallback order.|


### getFallbackIdOrderFromConvertedAmount

*Returns the ID of the fallback order for a given converted amount.*


```solidity
function getFallbackIdOrderFromConvertedAmount(uint256 _amount) external view returns (uint48 currentId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_amount`|`uint256`|The converted amount to get the fallback order ID for.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`currentId`|`uint48`|The ID of the fallback order. This function should not be called on-chain as it is expensive.|


### getFallbackIdOrder

*Returns the current fallback ID order for a given amount.*


```solidity
function getFallbackIdOrder(uint256 _amount) external view returns (uint48 currentId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_amount`|`uint256`|The amount to get the fallback ID order for.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`currentId`|`uint48`|The current fallback ID order. This function should not be called on-chain as it is expensive.|


### getQueueHead

*Returns the head of the sell orders queue.*


```solidity
function getQueueHead() public view returns (uint48);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint48`|The head of the sell orders queue as a uint48 value.|


### getQueueTail

*Returns the tail of the sell orders queue.*


```solidity
function getQueueTail() public view returns (uint48);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint48`|The tail of the sell orders queue.|


### getQueueOrder

*Returns the order at the specified index in the sellOrders queue.*


```solidity
function getQueueOrder(uint48 _orderId) public view returns (CSAQueueLib.Order memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_orderId`|`uint48`|The ID of the order to retrieve.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`CSAQueueLib.Order`|The order at the specified index in the sellOrders queue.|


### getAllQueueOrdersOwnedBy

*Returns an array of all queue orders owned by a specific address.*


```solidity
function getAllQueueOrdersOwnedBy(address owner) external view returns (uint48[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The address of the owner to filter the orders by.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint48[]`|An array of uint48 order IDs owned by the specified address.|


## Events
### Make
*Event emitted when a new order is created*


```solidity
event Make(address indexed buyer, uint256 amountIn);
```

### Take
*Event emitted when an order is executed*


```solidity
event Take(address indexed seller, uint256 amountIn, uint256 amountOut);
```

### Make
*Emitted when a user makes a buy order.*


```solidity
event Make(address indexed buyer, uint256 amountIn);
```

### Take
*Emitted when a user takes a sell order.*


```solidity
event Take(address indexed seller, uint256 amountIn, uint256 amountOut);
```

## Errors
### PriceLessThanZero
*Error thrown when the price of an order is less than zero*


```solidity
error PriceLessThanZero();
```

### NotEnoughBalance
*Error thrown when a user does not have enough balance to execute an order*


```solidity
error NotEnoughBalance();
```

### FallbackOrderNotEnough
*Error thrown when a fallback order is created but there is not enough balance to execute it*


```solidity
error FallbackOrderNotEnough();
```

### NoOrderIsEnoughToBeFallback
*Error thrown when a fallback order is requested but there are no orders available to use as fallback*


```solidity
error NoOrderIsEnoughToBeFallback();
```

### NotOrderOwner
*Error thrown when a user tries to execute an order they do not own*


```solidity
error NotOrderOwner();
```

### UserDoesNotHaveOrders
*Error thrown when a user tries to execute an order but they do not have any orders*


```solidity
error UserDoesNotHaveOrders();
```

### SellOrderTooSmall
*Error thrown when a sell order is too small to be executed*


```solidity
error SellOrderTooSmall();
```

### PriceLessThanZero
*Error thrown when the price of an order is less than or equal to zero.*


```solidity
error PriceLessThanZero();
```

### NotEnoughBalance
*Error thrown when a user does not have enough balance to execute an order.*


```solidity
error NotEnoughBalance();
```

### FallbackOrderNotEnough
*Error thrown when a fallback order does not have enough balance to be executed.*


```solidity
error FallbackOrderNotEnough();
```

### NoOrderIsEnoughToBeFallback
*Error thrown when there is no order that can be used as fallback.*


```solidity
error NoOrderIsEnoughToBeFallback();
```

### NotOrderOwner
*Error thrown when a user tries to execute an order that they do not own.*


```solidity
error NotOrderOwner();
```

### UserDoesNotHaveOrders
*Error thrown when a user does not have any orders.*


```solidity
error UserDoesNotHaveOrders();
```

### SellOrderTooSmall
*Error thrown when a sell order is too small to be executed.*


```solidity
error SellOrderTooSmall();
```

