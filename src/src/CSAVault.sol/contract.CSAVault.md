# CSAVault
**Inherits:**
AccessControl, Pausable, AutomationCompatibleInterface, ERC2771Context, ReentrancyGuard

**Author:**
Maurizio Murru https://github.com/akerbabber

*A smart contract that manages deposits and withdrawals with fees and supports pausing functionalities.
This contract is also compatible with Chainlink's Automation Interface for upkeep functionalities,
that can be set to check the balances and pause the contract if some funds have been somehow stolen.
This contract is made to interact with the CSAMarketplace contract, which is the only contract that can deposit funds.*


## State Variables
### token
*Address of the ERC20 token.*


```solidity
address public immutable token;
```


### PAUSER_ROLE
*Role identifier for pausers.*


```solidity
bytes32 public constant PAUSER_ROLE = keccak256("PAUSER_ROLE");
```


### MARKETPLACE_ROLE
*Role identifier for marketplace.*


```solidity
bytes32 public constant MARKETPLACE_ROLE = keccak256("MARKETPLACE_ROLE");
```


### FEE_SETTER_ROLE
*Role identifier for fee setters.*


```solidity
bytes32 public constant FEE_SETTER_ROLE = keccak256("FEE_SETTER_ROLE");
```


### fee
*Fee in percentage, 10000 = 100%, 1 = 0.01%.*


```solidity
uint256 public fee;
```


### FEE_PRECISION
*Precision for fee calculations.*


```solidity
uint256 public constant FEE_PRECISION = 10000;
```


### balances
*Mapping to store balances of users.*


```solidity
mapping(address => uint256) public balances;
```


### isOnUsersList
*Mapping to store user's data.*


```solidity
mapping(address => User) public isOnUsersList;
```


### usersList
*Array to store users addresses.*


```solidity
address[] public usersList;
```


## Functions
### constructor

*Contract constructor.*


```solidity
constructor(address _token, uint256 _fee, address trustedForwarder) ERC2771Context(trustedForwarder);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_token`|`address`|Address of the ERC20 token.|
|`_fee`|`uint256`|Initial fee percentage.|
|`trustedForwarder`|`address`||


### pause

*Allows pausers to pause the contract.*


```solidity
function pause() external;
```

### unpause

*Allows pausers to unpause the contract.*


```solidity
function unpause() external;
```

### deposit

*Allows marketplace to deposit on behalf of users.*


```solidity
function deposit(address[] calldata owners, uint256[] calldata _amounts) external whenNotPaused nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owners`|`address[]`|Addresses of the owners.|
|`_amounts`|`uint256[]`|Corresponding amounts to deposit.|


### withdraw

*Allows users to withdraw their balances.*


```solidity
function withdraw(uint256 _amount) external whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_amount`|`uint256`|Amount to withdraw.|


### withdrawFee

*Allows admins to withdraw fees.*


```solidity
function withdrawFee(uint256 _amount) external whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_amount`|`uint256`|Amount of fees to withdraw.|


### emergencyWithdraw

*Allows admins to perform emergency withdrawals.*


```solidity
function emergencyWithdraw(uint256 _amount, address _to) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_amount`|`uint256`|Amount to withdraw.|
|`_to`|`address`|Address to send the withdrawn amount to.|


### recoverUserFunds


```solidity
function recoverUserFunds(address _user, address _to) external;
```

### checkUpkeep

*Checks if the contract balance is enough to continue operating.*


```solidity
function checkUpkeep(bytes calldata checkData) external view returns (bool upkeepNeeded, bytes memory performData);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`checkData`|`bytes`|Data to check upkeep.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`upkeepNeeded`|`bool`|Flag indicating if upkeep is needed.|
|`performData`|`bytes`|Data to perform upkeep.|


### performUpkeep

*Pauses the contract in case of emergency.*


```solidity
function performUpkeep(bytes calldata performData) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`performData`|`bytes`|Data to perform upkeep.|


### setFee

*Allows admins to set the fee percentage.*


```solidity
function setFee(uint256 _fee) public onlyRole(FEE_SETTER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_fee`|`uint256`|New fee percentage.|


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

## Events
### Deposit
*Event emitted when a deposit is made.*


```solidity
event Deposit(address indexed _address, uint256 _amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_address`|`address`|Address of the user who deposited.|
|`_amount`|`uint256`|Amount deposited.|

### Withdraw
*Event emitted when a withdrawal is made.*


```solidity
event Withdraw(address indexed _address, uint256 _amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_address`|`address`|Address of the user who withdrew.|
|`_amount`|`uint256`|Amount withdrawn.|

## Errors
### NotPauser
*Error to show when the caller is not a pauser.*


```solidity
error NotPauser(address _address);
```

### NotMarketplace
*Error to show when the caller is not a marketplace.*


```solidity
error NotMarketplace(address _address);
```

### NotAdmin
*Error to show when the caller is not an admin.*


```solidity
error NotAdmin(address _address);
```

## Structs
### User
*User struct to manage user data.*


```solidity
struct User {
    uint160 index;
    bool isOnList;
}
```

