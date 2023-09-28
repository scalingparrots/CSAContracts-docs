# CSAOracle
**Inherits:**
[CSAFootballerFinder](/src/CSAFootballerFinder.sol/contract.CSAFootballerFinder.md), Ownable


## State Variables
### footballerScores

```solidity
mapping(uint256 => uint24[]) public footballerScores;
```


### nextMatchTimestamps

```solidity
mapping(uint256 => uint32[]) public nextMatchTimestamps;
```


### timestamps

```solidity
uint256[] public timestamps;
```


### MAX_FOOTBALLERS

```solidity
uint256 constant MAX_FOOTBALLERS = 500;
```


### gelatoSender

```solidity
address public gelatoSender;
```


## Functions
### setGelatoSender


```solidity
function setGelatoSender(address _gelatoSender) external onlyOwner;
```

### saveScoreBundle


```solidity
function saveScoreBundle(uint24[] calldata _scores, uint32[] calldata _nextMatchTimestamps) external;
```

### getScoreBundle


```solidity
function getScoreBundle(uint256 timestamp) external view returns (uint24[] memory);
```

### lastUpdated


```solidity
function lastUpdated() external view returns (uint256);
```

## Events
### FootballerScoreBundleSaved

```solidity
event FootballerScoreBundleSaved(uint256 timestamp, uint24[] scores, uint32[] nextMatchTimestamps);
```

## Errors
### InvalidScoreBundle

```solidity
error InvalidScoreBundle();
```

### TooManyFootballers

```solidity
error TooManyFootballers();
```

### FootballerNotFound

```solidity
error FootballerNotFound();
```

### NotGelato

```solidity
error NotGelato();
```

