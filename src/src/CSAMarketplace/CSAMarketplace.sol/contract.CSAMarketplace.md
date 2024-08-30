# CSAMarketplace
**Inherits:**
Initializable, AccessControlUpgradeable, UUPSUpgradeable, ERC2771ContextUpgradeable, [CSAMarketplaceStorageContract](/src/CSAMarketplace/CSAMarketplaceStorageContract.sol/contract.CSAMarketplaceStorageContract.md)

**Author:**
Maurizio Murru https://github.com/akerbabber

*This contract implements a marketplace for buying and selling CSAT tokens.
CSAT can be exchanged with a token chosen at deployment time.
It uses a queue data structure to match sell orders with buy orders.
The price of the token is determined by two Chainlink oracles: EUR/USD and Token/USD.
The contract also has a vault to store the tokens and a fee system for the marketplace.*


## Functions
### constructor

*Constructor function for the CSAMarketplace contract.*


```solidity
constructor(
    address _csat,
    address _tokenOut,
    AggregatorV3Interface _EURUSDOracle,
    AggregatorV3Interface _TokenUSDOracle,
    address _vault,
    uint256 _minBuyAmount,
    uint256 _dustAmount,
    address _trustedForwarder
)
    ERC2771ContextUpgradeable(_trustedForwarder)
    CSAMarketplaceStorageContract(_csat, _tokenOut, _EURUSDOracle, _TokenUSDOracle, _vault, _minBuyAmount, _dustAmount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_csat`|`address`|The address of the CSAT token contract.|
|`_tokenOut`|`address`|The address of the token contract exchanged for CSAT.|
|`_EURUSDOracle`|`AggregatorV3Interface`|The address of the EUR/USD price oracle contract.|
|`_TokenUSDOracle`|`AggregatorV3Interface`|The address of the token/USD price oracle contract.|
|`_vault`|`address`|The address of the CSAVault contract.|
|`_minBuyAmount`|`uint256`||
|`_dustAmount`|`uint256`||
|`_trustedForwarder`|`address`|The address of the trusted EIP-2771 forwarder.|


### initialize

*Initialize function for the CSAMarketplace contract.*


```solidity
function initialize(uint16 _priceFee) public initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_priceFee`|`uint16`|The fee percentage charged on the token price.|


### _authorizeUpgrade

*Reverts if called by any account other than an upgrader.*


```solidity
function _authorizeUpgrade(address newImplementation) internal override onlyRole(UPGRADER_ROLE);
```

### makeCsatSellOrder

The sell order will only be created if the amount is greater than or equal to 5 ether.

The CSAT tokens will be transferred from the seller's account to the marketplace contract.

Emits a Make event with the seller's address and the amount of CSAT tokens.

Uses the CSAQueueLib library to add the sell order to the sellOrders queue.

*Creates a sell order for the specified amount of CSAT tokens.*


```solidity
function makeCsatSellOrder(uint256 _amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_amount`|`uint256`|The amount of CSAT tokens to sell.|


### cancelCsatSellOrder

*Cancels a sell order for CSA tokens in the CSA marketplace.*


```solidity
function cancelCsatSellOrder(uint48 _orderId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_orderId`|`uint48`|The ID of the sell order to be cancelled. Emits a [NotOrderOwner](/src/CSAMarketplace/CSAMarketplace.sol/contract.CSAMarketplace.md#notorderowner) error if the caller is not the owner of the sell order.|


### buy

*This function allows a user to buy CSAT from the marketplace by matching the user's buy order with the first sell order in the queue.
If the first sell order is not enough, it will look up for the second and repeat up until the fourth.
If up until the fourth is not enough, a fallback id can be supplied to finish matching the order.*


```solidity
function buy(uint256 _amountIn, uint48 _fallbackId) external returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_amountIn`|`uint256`|The amount of tokens to buy.|
|`_fallbackId`|`uint48`|The fallback id to use if the first four sell orders are not enough to match the buy order.|


### buyFor

*This function allows a user to buy CSAT for another address from the marketplace by matching the user's buy order with the first sell order in the queue.
If the first sell order is not enough, it will look up for the second and repeat up until the fourth.
If up until the fourth is not enough, a fallback id can be supplied to finish matching the order.*


```solidity
function buyFor(uint256 _amountIn, uint48 _fallbackId, address receiver) external returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_amountIn`|`uint256`|The amount of tokens to buy.|
|`_fallbackId`|`uint48`|The fallback id to use if the first four sell orders are not enough to match the buy order.|
|`receiver`|`address`|The address to receive the CSAT tokens.|


### _buy

*Internal function to execute a buy order.*


```solidity
function _buy(uint256 _amountIn, uint48 _fallbackId, address receiver) internal returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_amountIn`|`uint256`|The amount of tokens to buy.|
|`_fallbackId`|`uint48`|The fallback id to use if the first four sell orders are not enough to match the buy order.|
|`receiver`|`address`|The address to receive the CSAT tokens.|


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


### setFee

This function can only be called by an admin.

*Sets the fee percentage charged on the token price.*


```solidity
function setFee(uint16 _priceFee) external onlyRole(FEE_SETTER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_priceFee`|`uint16`|The new fee percentage to set.|


### _checkExecutionLimit

This function is used to check if the user has reached the limit of daily orders.
This measure is in place to prevent users from spamming the marketplace with orders, leveraging the gasless feature.

*Checks if an address has not reached the execution limit.*


```solidity
function _checkExecutionLimit(address _user) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_user`|`address`|The address to check the execution limit for.|


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
### Make
*Event emitted when a new order is created*


```solidity
event Make(address indexed buyer, uint256 amountIn);
```

### Take
*Event emitted when an order is executed*


```solidity
event Take(address indexed seller, uint256 amountIn, uint256 amountOut, uint256[] amountsCSAT, address[] owners);
```

### CancelOrder

```solidity
event CancelOrder(address indexed owner, uint256 amount);
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

### ExecutionLimitReached

```solidity
error ExecutionLimitReached(address user);
```

### BuyOrderTooSmall
*Error thrown when a buy order is too small to be executed*


```solidity
error BuyOrderTooSmall();
```

## Structs
### BuyUintHelper

```solidity
struct BuyUintHelper {
    uint256 amountOut;
    uint256 unmatchedAmount;
    uint256 remainder;
    uint256 index;
    uint48 currentId;
    uint256 tokensToVault;
}
```

