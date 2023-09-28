# CSAEngine
**Inherits:**
AccessControl


## State Variables
### oracle

```solidity
CSAOracle public oracle;
```


### csac

```solidity
CSAC public csac;
```


### positions

```solidity
mapping(uint256 => Position) public positions;
```


### lastPositionId

```solidity
uint256 public lastPositionId;
```


### balances

```solidity
mapping(address => uint256) public balances;
```


### halted

```solidity
mapping(uint256 => Halted[]) public halted;
```


### HALTER_ROLE

```solidity
bytes32 public constant HALTER_ROLE = keccak256("HALTER_ROLE");
```


## Functions
### constructor


```solidity
constructor(address _oracle, address _halter, CSAC _csac);
```

### getFootballerScoreAndNextMatch


```solidity
function getFootballerScoreAndNextMatch(uint24 _footballerId) public view returns (uint24, uint32);
```

### openPosition


```solidity
function openPosition(uint24 _footballerId, Direction _direction, uint256 _amount) external;
```

### isFootballerPlayingToday


```solidity
function isFootballerPlayingToday(uint32 _nextMatch) public view returns (bool);
```

### isFootballerPlayingTodayById


```solidity
function isFootballerPlayingTodayById(uint24 _footballerId) public view returns (bool);
```

### calculateLongPnl


```solidity
function calculateLongPnl(uint256 _amount, uint24 _entryScore, uint24 _currentScore) public pure returns (uint256);
```

### calculateShortPnl


```solidity
function calculateShortPnl(uint256 _amount, uint24 _entryScore, uint24 _currentScore) public pure returns (uint256);
```

### calculatePnL


```solidity
function calculatePnL(Position memory _position) public view returns (uint256);
```

### calculateHaltedPnL


```solidity
function calculateHaltedPnL(Position memory _position, Halted memory _halt) public pure returns (uint256);
```

### closePosition


```solidity
function closePosition(uint256 _positionId) external;
```

### haltFootballer


```solidity
function haltFootballer(uint24 _footballerId) external;
```

### haltFootballers


```solidity
function haltFootballers(uint24[] calldata _footballerIds) external;
```

### unHaltFootballer


```solidity
function unHaltFootballer(uint24 _footballerId) external;
```

### unHaltFootballers


```solidity
function unHaltFootballers(uint24[] calldata _footballerIds) external;
```

### setOracle


```solidity
function setOracle(address _oracle) external;
```

### getHalted


```solidity
function getHalted(uint24 _footballerId) public view returns (Halted[] memory);
```

### isHalted


```solidity
function isHalted(uint24 _footballerId) public view returns (bool);
```

### wasHalted


```solidity
function wasHalted(uint24 _footballerId, uint32 timestamp) public view returns (bool, Halted memory);
```

### importPositions


```solidity
function importPositions(
    uint256[] calldata _amounts,
    address[] calldata _owners,
    uint32[] calldata _openTimestamps,
    uint24[] calldata _footballerIds,
    uint24[] calldata _entryScores,
    Direction[] calldata _directions
) external;
```

### positionsOwnedBy


```solidity
function positionsOwnedBy(address _owner) external view returns (uint256[] memory);
```

## Events
### OpenPosition

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

### FootballerPlayingToday

```solidity
error FootballerPlayingToday(uint24 footballerId);
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

## Structs
### Position

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

```solidity
enum Direction {
    LONG,
    SHORT
}
```

