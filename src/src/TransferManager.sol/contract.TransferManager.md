# TransferManager
**Inherits:**
Context, ERC2771Context

**Author:**
Maurizio Murru https://github.com/akerbabber

*A smart contract that manages token transfers and permits.
This contract is used to limit the number of uses per day for each user.
It also checks if the user is whitelisted.*

*This contract uses the ERC2771Context contract to handle meta transactions. And enables the use of
USDC permit function in conjunction with meta-txs.*


## State Variables
### useLimiter
*Mapping to store the use limiter for each user.*


```solidity
mapping(address => TransferManagerUseLimiterStruct) public useLimiter;
```


### csat
*CSAT contract address.*


```solidity
CSAT public immutable csat;
```


## Functions
### constructor

Constructor for the TransferManager contract.

*All variables are immutable, as such their values are hardcoded into the contract code*

*Changing them requires an upgrade of the contract*


```solidity
constructor(CSAT _csat, address _trustedForwarder) ERC2771Context(_trustedForwarder);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_csat`|`CSAT`|Address of the CSAT token contract|
|`_trustedForwarder`|`address`|Address of the trusted forwarder|


### permit

Function to permit a spender to spend a certain amount of tokens on behalf of the token owner.
This function is used to enable the use of the USDC permit function in conjunction with meta-txs.


```solidity
function permit(address _token, address _spender, uint256 _amount, uint256 _deadline, uint8 v, bytes32 r, bytes32 s)
    external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_token`|`address`|Address of the token|
|`_spender`|`address`|Address of the spender|
|`_amount`|`uint256`|Amount of tokens|
|`_deadline`|`uint256`|Expiration time for the permit|
|`v`|`uint8`|Signature v|
|`r`|`bytes32`|Signature r|
|`s`|`bytes32`|Signature s|


### permitAndTransfer

Function to transfer tokens from the caller to another address.


```solidity
function permitAndTransfer(
    address _token,
    address _to,
    uint256 _amount,
    uint256 _deadline,
    uint8 v,
    bytes32 r,
    bytes32 s
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_token`|`address`|Address of the token|
|`_to`|`address`|Address of the recipient|
|`_amount`|`uint256`|Amount of tokens|
|`_deadline`|`uint256`|Expiration time for the permit|
|`v`|`uint8`|Signature v|
|`r`|`bytes32`|Signature r|
|`s`|`bytes32`|Signature s|


### _isWhitelisted

Function to check if the user is whitelisted.


```solidity
function _isWhitelisted(address _account) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_account`|`address`|Address of the user|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if the user is whitelisted, false otherwise|


### _msgSender


```solidity
function _msgSender() internal view override(Context, ERC2771Context) returns (address sender);
```

### _msgData


```solidity
function _msgData() internal view override(Context, ERC2771Context) returns (bytes calldata);
```

### _contextSuffixLength


```solidity
function _contextSuffixLength() internal view override(Context, ERC2771Context) returns (uint256);
```

### _checkExecutionLimit

Function to check if the user has exceeded the limit of 25 uses per day.


```solidity
function _checkExecutionLimit(address _user) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_user`|`address`|Address of the user|


## Events
### Transfer
*Event emitted when a transfer is executed.*


```solidity
event Transfer(address indexed token, address indexed from, address indexed to, uint256 amount);
```

## Errors
### NotWhitelisted
*Error to show when the caller is not whitelisted.*


```solidity
error NotWhitelisted(address _address);
```

### ExecutionLimitReached
*Error to show when the user has exceeded the limit of 25 uses per day.*


```solidity
error ExecutionLimitReached(address _user);
```

## Structs
### TransferManagerUseLimiterStruct
*Struct to store the first use timestamp and the number of uses per day for each user.
This is used to limit the number of uses per day for each user.*


```solidity
struct TransferManagerUseLimiterStruct {
    uint40 firstUseTodayTimestamp;
    uint16 usesToday;
}
```

