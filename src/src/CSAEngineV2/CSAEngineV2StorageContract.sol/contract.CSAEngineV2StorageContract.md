# CSAEngineV2StorageContract
**Author:**
Maurizio Murru https://github.com/akerbabber

*This contract is used to store the storage layout of the CSAEngineV2 contract.*

*It includes the storage layout of the CSAEngineV2 contract and the necessary getter functions.*

*It also includes the necessary structs and enums used in the CSAEngineV2 contract.*

*This contract is used to avoid memory slot collisions with the CSAEngineV2 contract.*


## State Variables
### oracle
Address of the CSAOracleV2 contract which provides footballer data.


```solidity
CSAOracleV2 public immutable oracle;
```


### csat
Address of the CSAT contract which is a burnable and mintable token.


```solidity
CSAT public immutable csat;
```


### HALTER_ROLE
Role identifier for halter role, capable of halting and un-halting footballers.


```solidity
bytes32 public constant HALTER_ROLE = keccak256("HALTER_ROLE");
```


### UPGRADER_ROLE
Role identifier for upgrader role, capable of upgrading the contract.


```solidity
bytes32 public constant UPGRADER_ROLE = keccak256("UPGRADER_ROLE");
```


### CSAEngineV2LastPositionIdStorageLocation
The storage slot for the last position ID.


```solidity
bytes32 private constant CSAEngineV2LastPositionIdStorageLocation =
    keccak256(abi.encode(uint256(keccak256("CSA.storage.CSAEngineV2LastPositionId")) - 1)) & ~bytes32(uint256(0xff));
```


### CSAEngineV2PositionsStorageLocation
The storage slot for the positions.


```solidity
bytes32 private constant CSAEngineV2PositionsStorageLocation =
    keccak256(abi.encode(uint256(keccak256("CSA.storage.CSAEngineV2Positions")) - 1)) & ~bytes32(uint256(0xff));
```


### CSAEngineV2BalancesStorageLocation
The storage slot for the balances.


```solidity
bytes32 private constant CSAEngineV2BalancesStorageLocation =
    keccak256(abi.encode(uint256(keccak256("CSA.storage.CSAEngineV2Balances")) - 1)) & ~bytes32(uint256(0xff));
```


### CSAEngineV2HaltedStorageLocation
The storage slot for the halted footballers.


```solidity
bytes32 private constant CSAEngineV2HaltedStorageLocation =
    keccak256(abi.encode(uint256(keccak256("CSA.storage.CSAEngineV2Halted")) - 1)) & ~bytes32(uint256(0xff));
```


## Functions
### constructor

*Constructor function for CSAEngineV2 contract.*


```solidity
constructor(address _oracle, CSAT _csat);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_oracle`|`address`|The address of the CSAOracleV2 contract.|
|`_csat`|`CSAT`|The address of the CSAT contract.|


### _getCSAEngineV2LastPositionIdStorage

*This function is used to get the CSAEngineV2LastPositionIdStorage storage pointer.*


```solidity
function _getCSAEngineV2LastPositionIdStorage() internal pure returns (CSAEngineV2LastPositionIdStorage storage $);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`$`|`CSAEngineV2LastPositionIdStorage`|The storage pointer to the CSAEngineV2LastPositionIdStorage.|


### _getCSAEngineV2PositionsStorage

*This function is used to get the CSAEngineV2PositionsStorage storage pointer.*


```solidity
function _getCSAEngineV2PositionsStorage() internal pure returns (CSAEngineV2PositionsStorage storage $);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`$`|`CSAEngineV2PositionsStorage`|The storage pointer to the CSAEngineV2PositionsStorage.|


### _getCSAEngineV2BalancesStorage

*This function is used to get the CSAEngineV2BalancesStorage storage pointer.*


```solidity
function _getCSAEngineV2BalancesStorage() internal pure returns (CSAEngineV2BalancesStorage storage $);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`$`|`CSAEngineV2BalancesStorage`|The storage pointer to the CSAEngineV2BalancesStorage.|


### _getCSAEngineV2HaltedStorage

*This function is used to get the CSAEngineV2HaltedStorage storage pointer.*


```solidity
function _getCSAEngineV2HaltedStorage() internal pure returns (CSAEngineV2HaltedStorage storage $);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`$`|`CSAEngineV2HaltedStorage`|The storage pointer to the CSAEngineV2HaltedStorage.|


### lastPositionId

*This function is used to get the Last Position ID.*


```solidity
function lastPositionId() public view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The ID of the last position.|


### positions

*This function is used to get the position details.*


```solidity
function positions(uint256 _positionId) external view returns (Position memory position);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_positionId`|`uint256`|The ID of the position.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`position`|`Position`|The position struct as a tuple.|


### balances

*This function is used to get the balance of an address.*


```solidity
function balances(address _address) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_address`|`address`|The address of the user.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The balance of the address.|


### halted

*This function is used to get the halted footballer details.*


```solidity
function halted(uint256 _footballerId, uint256 _index) external view returns (uint32, uint24, uint24, uint32);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_footballerId`|`uint256`|The ID of the footballer.|
|`_index`|`uint256`|The index of the halted footballer.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint32`|The halted struct as a tuple.|
|`<none>`|`uint24`||
|`<none>`|`uint24`||
|`<none>`|`uint32`||


## Structs
### CSAEngineV2LastPositionIdStorage
The ID of the last position opened.


```solidity
struct CSAEngineV2LastPositionIdStorage {
    uint256 lastPositionId;
}
```

### CSAEngineV2PositionsStorage
Struct to store the storage layout of the CSAEngineV2 contract.

Mapping to store positions with their ID.


```solidity
struct CSAEngineV2PositionsStorage {
    mapping(uint256 => Position) positions;
}
```

### CSAEngineV2BalancesStorage
Struct to store the storage layout of the CSAEngineV2 contract.

Mapping to store balances of addresses.


```solidity
struct CSAEngineV2BalancesStorage {
    mapping(address => uint256) balances;
}
```

### CSAEngineV2HaltedStorage
Struct to store the storage layout of the CSAEngineV2 contract.

Mapping to store information about halted footballers.


```solidity
struct CSAEngineV2HaltedStorage {
    mapping(uint256 => Halted[]) halted;
}
```

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

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of CSA tokens held in the position.|
|`owner`|`address`|The address of the owner of the position.|
|`openTimestamp`|`uint32`|The timestamp when the position was opened.|
|`footballerId`|`uint24`|The ID of the footballer associated with the position.|
|`entryScore`|`uint24`|The score of the footballer at the time the position was opened.|
|`direction`|`Direction`|The direction of the position, either Long or Short.|

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

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`timestamp`|`uint32`|The timestamp when the footballer was halted.|
|`footballerId`|`uint24`|The ID of the halted footballer.|
|`score`|`uint24`|The score of the halted footballer.|
|`unHaltedTimestamp`|`uint32`|The timestamp when the footballer was un-halted.|

## Enums
### Direction
*Enum representing the direction of the position, LONG or SHORT.*


```solidity
enum Direction {
    LONG,
    SHORT
}
```

