# CSAFootballerFinder

## State Variables
### footballerIdToPosition

```solidity
mapping(uint256 => uint256) footballerIdToPosition;
```


### N_FOOTBALLERS

```solidity
uint256 constant N_FOOTBALLERS = 391;
```


## Functions
### constructor


```solidity
constructor();
```

### findFootballerPosition


```solidity
function findFootballerPosition(uint256 footballerId) public view returns (uint256);
```

