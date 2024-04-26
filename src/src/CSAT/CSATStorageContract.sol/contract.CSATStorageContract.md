# CSATStorageContract
**Author:**
Maurizio Murru https://github.com/akerbabber

*This contract is the storage definition of the CSAT erc20 contract.*


## State Variables
### MINTER_ROLE
*MINTER_ROLE is a constant bytes32 variable that represents the role of a minter.*


```solidity
bytes32 public constant MINTER_ROLE = 0x9f2df0fed2c77648de5860a4cc508cd0818c85b8b8a1ab4ceeef8d981c8956a6;
```


### WHITELIST_ROLE
*WHITELIST_ROLE is a constant bytes32 variable that represents the role of a whitelister.*


```solidity
bytes32 public constant WHITELIST_ROLE = 0xdc72ed553f2544c34465af23b847953efeb813428162d767f9ba5f4013be6760;
```


### UPGRADER_ROLE
backend should be whitelister

*UPGRADER_ROLE is a constant bytes32 variable that represents the role of an upgrader,
an upgrader is enabled to upgrade the contract.*


```solidity
bytes32 public constant UPGRADER_ROLE = 0x189ab7a9244df0848122154315af71fe140f3db0fe014031783b0946b8c9d2e3;
```


### CSATStorageLocation
*CSATStorageLocation is a constant bytes32 variable that represents the storage location of the CSATStorage struct.*


```solidity
bytes32 private constant CSATStorageLocation = 0xb75b55d9ccc0b5bc765a13fbd2ff66896644535d0eac4b7bd8e35ee76d34bd00;
```


## Functions
### _getCSATStorage

*get the storage struct of the contract.*


```solidity
function _getCSATStorage() internal pure returns (CSATStorage storage $);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`$`|`CSATStorage`|The storage struct of the contract.|


## Structs
### CSATStorage
*CSATStorage is a struct that stores the whitelist of the contract.*


```solidity
struct CSATStorage {
    mapping(address => bool) whitelist;
}
```

