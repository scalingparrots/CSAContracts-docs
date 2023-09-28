# CSAEngineV2
**Inherits:**
AccessControl

**Author:**
Maurizio Murru https://github.com/akerbabber

This contract handles positions opened on footballer's scores. It interacts with the oracle to fetch footballer scores and allows users to open positions based on those scores. It allows admin to halt and un-halt footballers and to change the oracle address. It also provides various utility functions to calculate PnL, check if a footballer is updated after a match, and to check if a footballer is halted.

*This contract uses OpenZeppelin contracts for Access Control and Safe ERC20 operations. It also uses CSAOracleV2 for fetching footballer data and CSAC token for burning and minting operations.*


## State Variables
### oracle
Address of the CSAOracleV2 contract which provides footballer data.


```solidity
CSAOracleV2 public oracle;
```


### csac
Address of the CSAC contract which is a burnable and mintable token.


```solidity
CSAC public csac;
```


### positions
Mapping to store positions with their ID.


```solidity
mapping(uint256 => Position) public positions;
```


### lastPositionId
The ID of the last position opened.


```solidity
uint256 public lastPositionId;
```


### balances
Mapping to store balances of addresses.


```solidity
mapping(address => uint256) public balances;
```


### halted
Mapping to store information about halted footballers.


```solidity
mapping(uint256 => Halted[]) public halted;
```


### HALTER_ROLE
Role identifier for halter role, capable of halting and un-halting footballers.


```solidity
bytes32 public constant HALTER_ROLE = keccak256("HALTER_ROLE");
```


## Functions
### constructor

*Constructor function for CSAEngineV2 contract.*


```solidity
constructor(address _oracle, address _halter, CSAC _csac);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_oracle`|`address`|The address of the CSAOracleV2 contract.|
|`_halter`|`address`|The address of the account that has the HALTER_ROLE.|
|`_csac`|`CSAC`|The address of the CSAC contract.|


### getFootballer

*Returns the footballer information for a given footballer ID.*


```solidity
function getFootballer(uint24 _footballerId) public view returns (CSAOracleV2.Footballer memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_footballerId`|`uint24`|The ID of the footballer to retrieve information for.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`CSAOracleV2.Footballer`|A `CSAOracleV2.Footballer` struct containing the footballer's information.|


### getFootballerScore

*Returns the score of a footballer with the given ID.*


```solidity
function getFootballerScore(uint24 _footballerId) public view returns (uint24);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_footballerId`|`uint24`|The ID of the footballer to get the score for.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint24`|The score of the footballer.|


### getFootballerScoreAndNextMatch

*Returns the score and next match timestamp of a given footballer.*


```solidity
function getFootballerScoreAndNextMatch(uint24 _footballerId) public view returns (uint24, uint32);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_footballerId`|`uint24`|The ID of the footballer to retrieve information for.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint24`|A tuple containing the footballer's score and next match timestamp.|
|`<none>`|`uint32`||


### openPosition

*Opens a new position for a given footballer, direction and amount.*


```solidity
function openPosition(uint24 _footballerId, Direction _direction, uint256 _amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_footballerId`|`uint24`|The ID of the footballer to open the position for.|
|`_direction`|`Direction`|The direction of the position (LONG or SHORT).|
|`_amount`|`uint256`|The amount of tokens to use for the position. Requirements: - The amount must be at least 10 ether. - The footballer must not be halted. - The footballer must have score and nextMatchTimestamp data available. - There must be a spread, if the position is long the _entryScore must be 1% lower than the current score if the position is short the _entryScore must be 1% higher than the current score. - The footballer's score must have been updated after the last match, or 48 hours must have passed since the last match.|


### isFootballerScoreUpdatedAfterMatch

*Checks if a footballer's score has been updated after their last match.*


```solidity
function isFootballerScoreUpdatedAfterMatch(CSAOracleV2.Footballer memory _footballer) public view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_footballer`|`CSAOracleV2.Footballer`|The footballer to check.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|A boolean indicating whether the footballer's score has been updated after their last match.|


### isFootballerScoreUpdatedAfterMatchById

*Checks if the score of a footballer has been updated after a match by ID.*


```solidity
function isFootballerScoreUpdatedAfterMatchById(uint256 _footballerId) public view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_footballerId`|`uint256`|The ID of the footballer to check.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|A boolean indicating whether the score has been updated or not.|


### calculateLongPnl

*Calculates the profit or loss of a long position based on the amount, entry score, and current score.*


```solidity
function calculateLongPnl(uint256 _amount, uint24 _entryScore, uint24 _currentScore) public pure returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_amount`|`uint256`|The amount of the position.|
|`_entryScore`|`uint24`|The score at which the position was opened.|
|`_currentScore`|`uint24`|The current score of the position.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The profit or loss of the position.|


### calculateShortPnl

*Calculates the short position profit or loss based on the amount, entry score, and current score.*


```solidity
function calculateShortPnl(uint256 _amount, uint24 _entryScore, uint24 _currentScore) public pure returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_amount`|`uint256`|The amount of the short position.|
|`_entryScore`|`uint24`|The score at which the short position was opened.|
|`_currentScore`|`uint24`|The current score of the short position.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The profit or loss of the short position.|


### calculatePnL

*Calculates the profit or loss of a given position based on the current footballer score.*


```solidity
function calculatePnL(Position memory _position) public view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_position`|`Position`|The position to calculate the PnL for.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The profit or loss of the position.|


### calculateHaltedPnL

*Calculates the profit and loss (PnL) of a halted position.*


```solidity
function calculateHaltedPnL(Position memory _position, Halted memory _halt) public pure returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_position`|`Position`|The position to calculate the PnL for.|
|`_halt`|`Halted`|The halted state of the position.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The PnL of the position.|


### closePosition

*Closes a position by deleting it from the positions mapping and emitting a ClosePosition event.
If the position is halted, calculates the PnL based on the halt information.
If the position is not halted, checks if the footballer score has been updated after the last match and calculates the PnL accordingly.
Transfers the PnL to the position owner by minting new tokens to their address.*


```solidity
function closePosition(uint256 _positionId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_positionId`|`uint256`|The ID of the position to be closed.|


### haltFootballer

*Halts a footballer by adding a new Halted struct to the halted mapping.*


```solidity
function haltFootballer(uint24 _footballerId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_footballerId`|`uint24`|The ID of the footballer to be halted. Requirements: - The caller must have the Halter role. - The footballer must not already be halted.|


### haltFootballers

Only users with the `HALTER_ROLE` can call this function.

If a footballer is already halted, the function will revert.

*Halts the specified footballers by adding them to the `halted` mapping.*


```solidity
function haltFootballers(uint24[] calldata _footballerIds) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_footballerIds`|`uint24[]`|An array of uint24 representing the IDs of the footballers to be halted.|


### unHaltFootballer

*Unhalts a footballer by updating the unHaltedTimestamp of the latest halt record.*


```solidity
function unHaltFootballer(uint24 _footballerId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_footballerId`|`uint24`|The ID of the footballer to unhalt. Requirements: - The caller must have the halter role. - The footballer must be currently halted.|


### unHaltFootballers

Only users with the Halter role can call this function.

If a footballer is not currently halted, it will revert with FootballerNotHalted error.

*Unhalt the specified footballers by updating their unHaltedTimestamp.*


```solidity
function unHaltFootballers(uint24[] calldata _footballerIds) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_footballerIds`|`uint24[]`|An array of uint24 representing the IDs of the footballers to unhalt.|


### setOracle

*Sets the address of the oracle contract.*


```solidity
function setOracle(address _oracle) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_oracle`|`address`|The address of the CSAOracleV2 contract. Requirements: - The caller must have the DEFAULT_ADMIN_ROLE.|


### getHalted

*Returns an array of Halted structs for a given footballer ID.*


```solidity
function getHalted(uint24 _footballerId) public view returns (Halted[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_footballerId`|`uint24`|The ID of the footballer to retrieve halted information for.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Halted[]`|An array of Halted structs containing information about halted periods for the given footballer.|


### isHalted

*Checks if a footballer is currently halted.*


```solidity
function isHalted(uint24 _footballerId) public view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_footballerId`|`uint24`|The ID of the footballer to check.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|A boolean indicating whether the footballer is currently halted or not.|


### wasHalted

*Checks if a footballer was halted at a given timestamp and returns the halted information.*


```solidity
function wasHalted(uint24 _footballerId, uint32 timestamp) public view returns (bool, Halted memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_footballerId`|`uint24`|The ID of the footballer to check.|
|`timestamp`|`uint32`|The timestamp to check against.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|A tuple containing a boolean indicating if the footballer was halted at the given timestamp and the halted information.|
|`<none>`|`Halted`||


### importPositions

*Imports an array of positions into the contract.*


```solidity
function importPositions(Position[] calldata _positions) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_positions`|`Position[]`|An array of Position structs to be imported.|


### positionsOwnedBy

*Returns an array of position IDs owned by the given address.*


```solidity
function positionsOwnedBy(address _owner) external view returns (uint256[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_owner`|`address`|The address to check for owned positions.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256[]`|An array of position IDs owned by the given address.|


## Events
### OpenPosition
Event emitted when a position is opened.


```solidity
event OpenPosition(
    uint256 indexed positionId,
    uint256 indexed amount,
    address indexed owner,
    uint32 openTimestamp,
    uint24 footballerId,
    uint24 entryScore,
    Direction direction
);
```

### ClosePosition
Event emitted when a position is closed.


```solidity
event ClosePosition(
    uint256 indexed positionId,
    uint256 indexed amount,
    address indexed owner,
    uint32 openTimestamp,
    uint24 footballerId,
    uint24 entryScore,
    Direction direction
);
```

### WithdrawBalance
Event emitted when balance is withdrawn.


```solidity
event WithdrawBalance(address indexed owner, uint256 amount);
```

## Errors
### NotAdmin

```solidity
error NotAdmin(address from);
```

### NotHalter

```solidity
error NotHalter(address from);
```

### NotPositionOwner

```solidity
error NotPositionOwner(address from, uint256 positionId);
```

### PositionNotFound

```solidity
error PositionNotFound(uint256 positionId);
```

### FootballerYetToReceiveScoreUpdateFromLastMatch

```solidity
error FootballerYetToReceiveScoreUpdateFromLastMatch(uint24 footballerId);
```

### NoDataForFootballer

```solidity
error NoDataForFootballer(uint24 footballerId);
```

### FootballerAlreadyHalted

```solidity
error FootballerAlreadyHalted(uint24 footballerId);
```

### FootballerNotHalted

```solidity
error FootballerNotHalted(uint24 footballerId);
```

### NothingToWithdraw

```solidity
error NothingToWithdraw(address from);
```

### OpenPositionLessThan10Ether

```solidity
error OpenPositionLessThan10Ether(address from, uint256 amount);
```

### FootballerIsHalted

```solidity
error FootballerIsHalted(uint24 footballerId);
```

## Structs
### Position
*Struct to represent a Position with necessary details.*


```solidity
struct Position {
    uint256 amount;
    address owner;
    uint32 openTimestamp;
    uint24 footballerId;
    uint24 entryScore;
    Direction direction;
}
```

### Halted
*Struct to represent a halted footballer with necessary details.*


```solidity
struct Halted {
    uint32 timestamp;
    uint24 footballerId;
    uint24 score;
    uint32 unHaltedTimestamp;
}
```

## Enums
### Direction
*Enum representing the direction of the position, LONG or SHORT.*


```solidity
enum Direction {
    LONG,
    SHORT
}
```

