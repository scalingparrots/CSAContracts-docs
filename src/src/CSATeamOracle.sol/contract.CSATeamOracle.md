# CSATeamOracle
**Inherits:**
AccessControl

**Author:**
Maurizio Murru https://github.com/akerbabber

*This contract functions as an oracle to store and manage information
related to footballers' scores and match timestamps. It leverages the
AccessControl contract from OpenZeppelin to manage access permissions and roles.*


## State Variables
### footballers
*Array to store footballers' scores.
The array is structured as follows: [footballerId1, score1, footballerId2, score2, ...]*


```solidity
uint24[] public footballers;
```


### indexValue
*The index value of the team.*


```solidity
Index[] public indexValue;
```


### oracle
*Address of the CSAOracleV2 contract to store the index value and make it available to the CSAEngineV2 contract.*


```solidity
CSAOracleV2 public immutable oracle;
```


### UPDATER_ROLE
*Role identifier for users who can update footballer scores.*


```solidity
bytes32 public constant UPDATER_ROLE = keccak256("UPDATER_ROLE");
```


## Functions
### constructor

*Initializes the contract setting the deployer as the initial admin.*


```solidity
constructor(uint24 _indexStartingValue, CSAOracleV2 _oracle);
```

### updateIndex

*Updates the footballers' scores and the index value of the team.*


```solidity
function updateIndex(uint24[] calldata _footballers, uint24 _indexValue) public;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_footballers`|`uint24[]`|Array of Footballer structs containing the ID and score of each footballer.|
|`_indexValue`|`uint24`|The index value of the team.|


### getFootballersStruct

*Returns the footballers' scores.*


```solidity
function getFootballersStruct() public view returns (Footballer[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Footballer[]`|Array of Footballer structs containing the ID and score of each footballer.|


### getFootballers

*Returns the footballers' RAW array.*


```solidity
function getFootballers() public view returns (uint24[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint24[]`|Array of footballers' IDs and scores.|


### getIndexValue


```solidity
function getIndexValue() public view returns (Index[] memory);
```

### getLastIndexValue


```solidity
function getLastIndexValue() public view returns (Index memory);
```

### _indexValueBetweenTwoDates


```solidity
function _indexValueBetweenTwoDates(uint256 start, uint256 end) internal view returns (Index[] memory);
```

### getProfitAndLoss


```solidity
function getProfitAndLoss() public view returns (int256);
```

### getProfitAndLossFromBeginning


```solidity
function getProfitAndLossFromBeginning() public view returns (int256);
```

### getProfitAndLossBetweenTwoDates


```solidity
function getProfitAndLossBetweenTwoDates(uint256 start, uint256 end) public view returns (int256);
```

### indexValueBetweenTwoDates


```solidity
function indexValueBetweenTwoDates(uint256 start, uint256 end) public view returns (Index[] memory);
```

### calculatePercentage


```solidity
function calculatePercentage(uint256 numerator, uint256 denominator) public pure returns (int256);
```

## Errors
### NotUpdater
*Custom error to throw if the caller does not have updater role.*


```solidity
error NotUpdater(address _address);
```

## Structs
### Footballer

```solidity
struct Footballer {
    uint24 id;
    uint24 score;
}
```

### Index

```solidity
struct Index {
    uint24 indexValue;
    uint32 timestamp;
}
```

