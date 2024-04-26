# CSAQueueLib
**Author:**
Maurizio Murru https://github.com/akerbabber

*A library that provides a basic Queue data structure with Order elements.
It provides functions to add, remove, update, and get elements from the Queue.
The Queue keeps track of the head and tail, and each Order has a link to its previous
and next Order, forming a doubly linked list.*


## State Variables
### MAX_ORDERS_REMOVABLE

```solidity
uint256 constant MAX_ORDERS_REMOVABLE = 10;
```


## Functions
### add

*Adds a new order to the end of the queue.*


```solidity
function add(Queue storage self, address _owner, uint256 _amount) internal returns (uint48);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`self`|`Queue`|The queue to add the order to.|
|`_owner`|`address`|The address of the order owner.|
|`_amount`|`uint256`|The amount of the order.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint48`|The new tail of the queue.|


### remove

*Removes an order from the queue.*


```solidity
function remove(Queue storage self, uint48 orderId) internal returns (Receipt memory receipt);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`self`|`Queue`|The queue to remove the order from.|
|`orderId`|`uint48`|The ID of the order to remove.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`receipt`|`Receipt`|The receipt of the removed order. Emits an [OrderNotFound](/src/CSAQueueLib.sol/library.CSAQueueLib.md#ordernotfound) event if the order is not found in the queue.|


### multipleRemove

This function requires that the orderIds are valid and subsequent.

This function requires that the number of orderIds is less than or equal to MAX_ORDERS_REMOVABLE.

This function returns an empty array if the first orderId is 0.

*Removes multiple orders from the queue by their orderIds.*


```solidity
function multipleRemove(Queue storage self, uint48[] memory orderIds) internal returns (Receipt[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`self`|`Queue`|The queue to remove orders from.|
|`orderIds`|`uint48[]`|The array of orderIds to remove.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Receipt[]`|An array of Receipt structs containing the owner and amount of each removed order.|


### removeTrailingZeros

*Removes trailing zeros from an array of uint48.*


```solidity
function removeTrailingZeros(uint48[] memory arr) internal pure returns (uint48[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`arr`|`uint48[]`|The input array to remove trailing zeros from.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint48[]`|A new array with the trailing zeros removed.|


### update

*Updates the amount of an order in the queue.*


```solidity
function update(Queue storage self, uint48 orderId, uint256 newAmount) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`self`|`Queue`|The queue to update.|
|`orderId`|`uint48`|The ID of the order to update.|
|`newAmount`|`uint256`|The new amount of the order.|


### head

*Returns the ID of the first order in the queue.*


```solidity
function head(Queue storage self) internal view returns (uint48);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`self`|`Queue`|The queue to get the head of.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint48`|The ID of the first order in the queue.|


### tail

*Returns the ID of the last order in the queue.*


```solidity
function tail(Queue storage self) internal view returns (uint48);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`self`|`Queue`|The queue to get the tail of.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint48`|The ID of the last order in the queue.|


### get

*Returns the order with the given ID.*


```solidity
function get(Queue storage self, uint48 orderId) internal view returns (Order memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`self`|`Queue`|The queue to get the order from.|
|`orderId`|`uint48`|The ID of the order to get.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Order`|The order with the given ID.|


### next

*Returns the ID of the order that comes after the given order in the queue.*


```solidity
function next(Queue storage self, uint48 orderId) internal view returns (uint48);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`self`|`Queue`|The queue to get the next order from.|
|`orderId`|`uint48`|The ID of the order to get the next order of.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint48`|The ID of the order that comes after the given order in the queue.|


### prev

*Returns the ID of the order that comes before the given order in the queue.*


```solidity
function prev(Queue storage self, uint48 orderId) internal view returns (uint48);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`self`|`Queue`|The queue to get the previous order from.|
|`orderId`|`uint48`|The ID of the order to get the previous order of.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint48`|The ID of the order that comes before the given order in the queue.|


### getOrders

*Returns an array of all orders in the queue.*


```solidity
function getOrders(Queue storage self) internal view returns (Order[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`self`|`Queue`|The queue to retrieve orders from.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Order[]`|An array of Order structs representing the orders in the queue.|


## Errors
### OrderNotFound
Custom errors


```solidity
error OrderNotFound(uint128 orderId);
```

### OrdersNotSubsequent

```solidity
error OrdersNotSubsequent(uint128 orderId);
```

### TooManyOrdersToRemove

```solidity
error TooManyOrdersToRemove(uint256 amount);
```

## Structs
### Order
*Represents an order in the Queue.*


```solidity
struct Order {
    address owner;
    uint48 next;
    uint48 prev;
    uint256 amount;
}
```

### Receipt
*Represents a receipt of an order in the Queue.*


```solidity
struct Receipt {
    address owner;
    uint256 amount;
}
```

### Queue
*Represents the Queue data structure.*


```solidity
struct Queue {
    uint48 head;
    uint48 tail;
    mapping(uint128 => Order) orders;
}
```

