# CSATBuyer
**Inherits:**
Initializable, AccessControlUpgradeable, UUPSUpgradeable, ERC2771ContextUpgradeable, ReentrancyGuardUpgradeable

**Author:**
Maurizio Murru https://github.com/akerbabber

*This contract is a buyer for CSAT tokens. It is able to buy CSAT tokens from the marketplace and from the swap in
a single transaction. It is able to calculate the optimal amount to buy from the marketplace and from the swap.
It is able to calculate the maximum amount that can be bought from the marketplace.
It is able to calculate the fallback id in the marketplace for a given amount.
It supports meta transactions and is upgradeable (UUPS).*


## State Variables
### MAX_N_ORDERS
*Maximum number of orders to check in the CSAMarketplace, if the requested amount is greater than the sum of the first
MAX_N_ORDERS, the remaining amount will be bought from the fallback order with the highest amount.
If even the fallback is not enough, it will rely on the CSASwap.*


```solidity
uint256 constant MAX_N_ORDERS = 4;
```


### MIN_AMOUNT_IN
*Minimum amount of USDC that can be sold for CSAT using CSATBuyer. This limit is in place to avoid
to make anti-economic making transactions with a very low amount of USDC. Otherwise the paymaster could
be drained to pay for the gas fees of the transaction.*


```solidity
uint256 constant MIN_AMOUNT_IN = 5e6;
```


### CSASWAP_ADDRESS
*Address of the CSASwap contract.*


```solidity
address public immutable CSASWAP_ADDRESS;
```


### CSAMARKETPLACE_ADDRESS
*Address of the CSAMarketplace contract.*


```solidity
address public immutable CSAMARKETPLACE_ADDRESS;
```


### USDC_ADDRESS
*Address of the USDC token contract.*


```solidity
address public immutable USDC_ADDRESS;
```


### UPGRADER_ROLE
*Role for the upgrader, he can upgrade the contract.*


```solidity
bytes32 public constant UPGRADER_ROLE = keccak256("UPGRADER_ROLE");
```


## Functions
### constructor

Constructor for the CSATBuyer contract.

*All variables are immutable, as such their values are hardcoded into the contract code*

*Changing them requires an upgrade of the contract*


```solidity
constructor(address _usdcAddress, address _csaswapAddress, address _csamarketplaceAddress, address _trustedForwarder)
    ERC2771ContextUpgradeable(_trustedForwarder);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_usdcAddress`|`address`|Address of the USDC token contract|
|`_csaswapAddress`|`address`|Address of the CSASwap contract|
|`_csamarketplaceAddress`|`address`|Address of the CSAMarketplace contract|
|`_trustedForwarder`|`address`|Address of the trusted forwarder|


### initialize

Initializes the CSATBuyer contract.


```solidity
function initialize() public initializer;
```

### _authorizeUpgrade

*Reverts if called by any account other than an upgrader.*


```solidity
function _authorizeUpgrade(address newImplementation) internal override onlyRole(UPGRADER_ROLE);
```

### maximumBuyOnMarketplace

Returns the maximum amount that can be bought from the marketplace in a single transaction.

*This function is expensive, do not call it onchain.*


```solidity
function maximumBuyOnMarketplace() public view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The maximum amount that can be bought from the marketplace.|


### getFallbackIdFromMarketplace

Returns the fallback id in the marketplace for a given amount.

*This function is expensive, do not call it onchain.*


```solidity
function getFallbackIdFromMarketplace(uint256 _amount) external view returns (uint48);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_amount`|`uint256`|The amount for which to get the fallback id.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint48`|The fallback id in the marketplace for the given amount.|


### getOptimalInput

Returns the optimal input to pass to the buyCsatFor() function, for the given amount.

*This function is expensive, do not call it onchain.*


```solidity
function getOptimalInput(uint256 _amount)
    external
    view
    returns (uint256 _amountMarketplace, uint256 _amountSwap, uint48 _fallbackId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_amount`|`uint256`|The amount for which to get the optimal input.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`_amountMarketplace`|`uint256`|The amount to buy from the marketplace.|
|`_amountSwap`|`uint256`|The amount to buy from the swap.|
|`_fallbackId`|`uint48`|The fallback id in the marketplace.|


### buyCsatFor

Buys CSAT tokens for the given amount.


```solidity
function buyCsatFor(uint256 _amountMarketplace, uint256 _amountSwap, uint48 _fallbackId, address _receiver)
    external
    nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_amountMarketplace`|`uint256`|The amount to buy from the marketplace.|
|`_amountSwap`|`uint256`|The amount to buy from the swap.|
|`_fallbackId`|`uint48`|The fallback id in the marketplace.|
|`_receiver`|`address`|The address that will receive the CSAT tokens.|


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
### BoughtCSAT
*Event emitted when CSAT tokens are bought trough the CSATBuyer contract.*


```solidity
event BoughtCSAT(
    address indexed _buyer,
    uint256 _amountMarketplace,
    uint256 _fallbackId,
    uint256 _amountSwap,
    address indexed _receiver
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_buyer`|`address`|The address of the buyer.|
|`_amountMarketplace`|`uint256`|The amount bought from the marketplace.|
|`_fallbackId`|`uint256`|The fallback id in the marketplace.|
|`_amountSwap`|`uint256`|The amount bought from the swap.|
|`_receiver`|`address`|The address that will receive the CSAT tokens.|

## Errors
### AmountTooLow
*Error emitted when the amount of USDC is too low to use the contract.*


```solidity
error AmountTooLow(uint256 _amount);
```

