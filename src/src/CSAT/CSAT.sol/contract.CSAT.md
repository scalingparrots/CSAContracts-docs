# CSAT
**Inherits:**
Initializable, ERC20Upgradeable, ERC20PermitUpgradeable, AccessControlUpgradeable, UUPSUpgradeable, ERC2771ContextUpgradeable, [CSATStorageContract](/src/CSAT/CSATStorageContract.sol/contract.CSATStorageContract.md)

**Author:**
Maurizio Murru https://github.com/akerbabber

*This contract is a standard ERC20 token with additional access controls.*

*This contract defines the roles and permissions for minting and whitelisting addresses.
It also includes events for adding and removing addresses from the whitelist, and for opting in and out of the whitelist.
Additionally, it defines error messages for when an address is not whitelisted, not a minter, not a whitelister, or not an admin.*


## Functions
### whitelist

*Whitelists an address to enable transfers to and from that address.*


```solidity
function whitelist(address _address) external view returns (bool);
```

### constructor

Disables the ability to call initialize functions. This is done to follow the UUPS upgradeable pattern.


```solidity
constructor(address trustedForwarder) ERC2771ContextUpgradeable(trustedForwarder);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`trustedForwarder`|`address`|The address of the trusted forwarder.|


### initialize

Initializes the contract, sets the token name and symbol, and assigns the admin role
to the _msgSender().


```solidity
function initialize(string calldata name, string calldata symbol) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|The name of the token.|
|`symbol`|`string`|The symbol of the token.|


### _authorizeUpgrade

*Reverts if called by any account other than an upgrader.*


```solidity
function _authorizeUpgrade(address newImplementation) internal override onlyRole(UPGRADER_ROLE);
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


### _update

*Overrides the internal function to add custom whitelist validation.*


```solidity
function _update(address from, address to, uint256 amount) internal override;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address.|
|`to`|`address`|The recipient address.|
|`amount`|`uint256`|The amount to be transferred.|


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
### AddToWhitelist
*AddToWhitelist is an event that is emitted when an address is added to the whitelist.*


```solidity
event AddToWhitelist(address indexed _address, address indexed _msgSender);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_address`|`address`|The address that was added to the whitelist.|
|`_msgSender`|`address`||

### RemoveFromWhitelist
*RemoveFromWhitelist is an event that is emitted when an address is removed from the whitelist.*


```solidity
event RemoveFromWhitelist(address indexed _address, address indexed _msgSender);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_address`|`address`|The address that was removed from the whitelist.|
|`_msgSender`|`address`||

## Errors
### NotWhitelisted
*NotWhitelisted is an error message that is thrown when an address is not whitelisted.*


```solidity
error NotWhitelisted(address _address);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_address`|`address`|The address that is not whitelisted.|

### NotMinter
*NotMinter is an error message that is thrown when an address is not a minter.*


```solidity
error NotMinter(address _address);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_address`|`address`|The address that is not a minter.|

### NotWhitelister
*NotWhitelister is an error message that is thrown when an address is not a whitelister.*


```solidity
error NotWhitelister(address _address);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_address`|`address`|The address that is not a whitelister.|

### NotAdmin
*NotAdmin is an error message that is thrown when an address is not an admin.*


```solidity
error NotAdmin(address _address);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_address`|`address`|The address that is not an admin.|

