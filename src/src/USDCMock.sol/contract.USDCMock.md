# USDCMock
**Inherits:**
ERC20, ERC20Permit


## Functions
### constructor


```solidity
constructor() ERC20("USDC", "US Dollar Coin") ERC20Permit("USDC");
```

### mint


```solidity
function mint(address to, uint256 amount) external;
```

### burn


```solidity
function burn(address from, uint256 amount) external;
```

### decimals


```solidity
function decimals() public pure override returns (uint8);
```

