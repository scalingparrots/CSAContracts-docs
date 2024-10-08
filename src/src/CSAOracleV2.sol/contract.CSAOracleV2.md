# CSAOracleV2
**Inherits:**
AccessControl

**Author:**
Maurizio Murru https://github.com/akerbabber

*This contract functions as an oracle to store and manage information
related to footballers' scores and match timestamps. It leverages the
AccessControl contract from OpenZeppelin to manage access permissions and roles.*


## State Variables
### footballers
*Mapping from playerId to Footballer struct, storing footballer data.*


```solidity
mapping(uint256 => Footballer) public footballers;
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
constructor();
```

### updateFootballerScore

*Allows to update the footballer’s score and match timestamps.
Can only be called by addresses with the UPDATER_ROLE.
Emits a FootballerScoreUpdated event.*


```solidity
function updateFootballerScore(
    uint256 _playerId,
    uint24 _score,
    uint32 _nextMatchTimestamp,
    uint32 _previousNextMatchTimestamp,
    uint32 _lastMatchTimestamp,
    uint40 _nextMatchId,
    uint40 _lastMatchId
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_playerId`|`uint256`|The unique identifier of the footballer.|
|`_score`|`uint24`|The updated score.|
|`_nextMatchTimestamp`|`uint32`|The updated timestamp of the next match.|
|`_previousNextMatchTimestamp`|`uint32`|The updated timestamp of the previous 'next match'.|
|`_lastMatchTimestamp`|`uint32`|The updated timestamp of the last match.|
|`_nextMatchId`|`uint40`|The updated ID of the next match.|
|`_lastMatchId`|`uint40`|The updated ID of the last match.|


### updateFootballerScoreBundle

*Allows to update the scores and match timestamps for multiple footballers.
Can only be called by addresses with the UPDATER_ROLE.
Emits multiple FootballerScoreUpdated events.*


```solidity
function updateFootballerScoreBundle(uint256[] calldata _playerIds, Footballer[] calldata _footballers) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_playerIds`|`uint256[]`|An array of player identifiers.|
|`_footballers`|`Footballer[]`|An array of Footballer structs containing the updated data.|


### getFootballer

*Returns the footballer data for a given player ID.*


```solidity
function getFootballer(uint256 _playerId) external view returns (Footballer memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_playerId`|`uint256`|The unique identifier of the footballer.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Footballer`|Footballer memory The footballer data.|


### getFootballers

*Returns the footballer data for a given array of player IDs.*


```solidity
function getFootballers(uint256[] calldata _playerIds) external view returns (Footballer[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_playerIds`|`uint256[]`|An array of unique identifiers of the footballers.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Footballer[]`|Footballer[] memory An array containing the footballer data for each provided ID.|


## Events
### FootballerScoreUpdated
*Event emitted when a footballer's score is updated.*


```solidity
event FootballerScoreUpdated(
    uint256 indexed playerId,
    uint24 score,
    uint32 nextMatchTimestamp,
    uint32 previousNextMatchTimestamp,
    uint32 lastMatchTimestamp,
    uint40 nextMatchId,
    uint40 lastMatchId
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`playerId`|`uint256`|The unique identifier of the footballer.|
|`score`|`uint24`|The updated score of the footballer.|
|`nextMatchTimestamp`|`uint32`|The updated timestamp of the next match.|
|`previousNextMatchTimestamp`|`uint32`||
|`lastMatchTimestamp`|`uint32`|The updated timestamp of the last match.|
|`nextMatchId`|`uint40`||
|`lastMatchId`|`uint40`||

## Errors
### NotUpdater
*Custom error to throw if the caller does not have updater role.*


```solidity
error NotUpdater(address _address);
```

## Structs
### Footballer
*Struct representing a Footballer.*


```solidity
struct Footballer {
    uint24 score;
    uint32 nextMatchTimestamp;
    uint32 previousNextMatchTimestamp;
    uint32 lastMatchTimestamp;
    uint40 nextMatchId;
    uint40 lastMatchId;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`score`|`uint24`|The score associated with the footballer.|
|`nextMatchTimestamp`|`uint32`|The timestamp of the footballer's next match.|
|`previousNextMatchTimestamp`|`uint32`|The timestamp of the previous 'next match' before the latest update.|
|`lastMatchTimestamp`|`uint32`|The timestamp of the footballer's last match.|
|`nextMatchId`|`uint40`||
|`lastMatchId`|`uint40`||

