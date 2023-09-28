# CSAT
**Inherits:**
ERC20, AccessControl

**Author:**
Maurizio Murru https://github.com/akerbabber

*This contract is a standard ERC20 token with additional access controls.*

*This contract defines the roles and permissions for minting and whitelisting addresses.
It also includes events for adding and removing addresses from the whitelist, and for opting in and out of the whitelist.
Additionally, it defines error messages for when an address is not whitelisted, not a minter, not a whitelister, or not an admin.*


## State Variables
### MINTER_ROLE
*MINTER_ROLE is a constant bytes32 variable that represents the role of a minter.*


```solidity
bytes32 public constant MINTER_ROLE = keccak256("MINTER_ROLE");
```


### WHITELIST_ROLE
*WHITELIST_ROLE is a constant bytes32 variable that represents the role of a whitelister.*


```solidity
bytes32 public constant WHITELIST_ROLE = keccak256("WHITELIST_ROLE");
```


### whitelist
*whitelist is a mapping that stores whether an address is whitelisted or not.*


```solidity
mapping(address => bool) public whitelist;
```


### isWhitelisted
*isWhitelisted is a boolean variable that represents whether the contract is currently in whitelist mode or not.*


```solidity
bool public isWhitelisted;
```


## Functions
### constructor

Initializes the contract, sets the token name and symbol, and assigns the admin role
to the msg.sender.


```solidity
constructor() ERC20("CSAT", "CSAT");
```

### mint

Mints `amount` tokens to address `to`.

*Can only be called by addresses with the `MINTER_ROLE`.*


```solidity
function mint(address to, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|Address to receive the minted tokens.|
|`amount`|`uint256`|Amount of tokens to mint.|


### burn

Burns `amount` tokens from address `from`.

*Can only be called by addresses with the `MINTER_ROLE`.*


```solidity
function burn(address from, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Address to burn the tokens from.|
|`amount`|`uint256`|Amount of tokens to burn.|


### addToWhitelist

Adds address `_address` to the whitelist.

*Can only be called by addresses with the `WHITELIST_ROLE`.*


```solidity
function addToWhitelist(address _address) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_address`|`address`|Address to be added to the whitelist.|


### removeFromWhitelist

Removes address `_address` from the whitelist.

*Can only be called by addresses with the `WHITELIST_ROLE`.*


```solidity
function removeFromWhitelist(address _address) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_address`|`address`|Address to be removed from the whitelist.|


### optOutWhitelist

Opt-out from the whitelist functionality.

*Can only be called by addresses with the `DEFAULT_ADMIN_ROLE`.*


```solidity
function optOutWhitelist() external;
```

### _beforeTokenTransfer

*Overrides the internal function to add custom whitelist validation.*


```solidity
function _beforeTokenTransfer(address from, address to, uint256 amount) internal override;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address.|
|`to`|`address`|The recipient address.|
|`amount`|`uint256`|The amount to be transferred.|


## Events
### WhitelistOptOut
*WhitelistOptOut is an event that is emitted when the contract exits whitelist mode.*


```solidity
event WhitelistOptOut(bool _isWhitelisted);
```

### WhitelistOptIn
*WhitelistOptIn is an event that is emitted when the contract enters whitelist mode.*


```solidity
event WhitelistOptIn(bool _isWhitelisted);
```

### AddToWhitelist
*AddToWhitelist is an event that is emitted when an address is added to the whitelist.*


```solidity
event AddToWhitelist(address indexed _address);
```

### RemoveFromWhitelist
*RemoveFromWhitelist is an event that is emitted when an address is removed from the whitelist.*


```solidity
event RemoveFromWhitelist(address indexed _address);
```

## Errors
### NotWhitelisted
*NotWhitelisted is an error message that is thrown when an address is not whitelisted.*


```solidity
error NotWhitelisted(address _address);
```

### NotMinter
*NotMinter is an error message that is thrown when an address is not a minter.*


```solidity
error NotMinter(address _address);
```

### NotWhitelister
*NotWhitelister is an error message that is thrown when an address is not a whitelister.*


```solidity
error NotWhitelister(address _address);
```

### NotAdmin
*NotAdmin is an error message that is thrown when an address is not an admin.*


```solidity
error NotAdmin(address _address);
```

