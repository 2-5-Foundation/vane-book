# Vane-Order

## Module Overview

This module defines callable functions for placing and managing orders. Users can place orders for products, and these orders are associated with both the payer and the seller. Order information is stored in the runtime's storage.## Usage
This module can be integrated into a Substrate runtime to enable decentralized e-commerce functionality. Users can place and manage orders for products registered in the system. The module interacts with other pallets and libraries to verify product existence and handle order placement and management.

Please note that the functionality of this module is dependent on the configuration and interactions with other pallets and modules within the Substrate runtime. It should be used in conjunction with other components to create a complete e-commerce system.

## Configuration

The behavior of this module can be customized through a configuration trait `Config`. The configuration trait specifies the runtime's requirements and includes the following traits:

- `frame_system::Config`: Required for system-related functionality.
- `vane_register::Config`: Required for interacting with the product registration system.
- `Currency<Self::AccountId>`: The currency type used for transactions in the runtime.

## Storage

1. **PayerOrder**:
   - **Description**: This storage item is an unbounded map that associates the payer's account with a vector of orders. Each order is represented by a struct `Order<T>`. Orders are indexed by the payer's account.
   - **Storage Type**: `StorageMap<_, Blake2_128Concat, T::AccountId, Vec<Order<T>>, ValueQuery>`

2. **PayeeOrderRef**:
   - **Description**: This storage item is an unbounded map that associates the seller's account with a vector of tuples. Each tuple contains the payer's account, an item ID, and an order number. This storage is used to reference orders for a specific payee.
   - **Storage Type**: `StorageMap<_, Blake2_128Concat, T::AccountId, Vec<(T::AccountId, u32, u32)>, ValueQuery>`

## Events

1. **OrderPlaced**:
   - **Description**: This event is emitted when an order is successfully placed. It includes information about the buyer, the item, the order number, and the seller.
   - **Event Fields**:
     - `buyer`: The account of the buyer who placed the order.
     - `item_id`: The ID of the product or item being ordered.
     - `order_no`: The order number associated with the placed order.
     - `seller`: The account of the seller from whom the order was placed.

2. **OrderCancelled**:
   - **Description**: This event is emitted when an order is cancelled.

## Errors

1. **ProductDontExist**:
   - **Description**: An error indicating that the requested product does not exist in the product registration system.
2. **UnexpectedError**:
   - **Description**: An error indicating an unexpected or system-level error.

## Callable Functions

### `place_order`

- **Parameters**:
  - `origin`: An `OriginFor<T>` representing the caller's origin.
  - `item_id`: A `u32` representing the ID of the item being ordered.
  - `seller_id`: A `T::AccountId` representing the account of the seller.

- **Description**:
  - This function allows a user to place an order for a specific item from a seller.
  - The caller's identity is verified, and the product associated with the seller is retrieved from the product registration system.
  - The function checks if the requested product exists.
  - If the product exists, it constructs a new order and associates it with both the payer and seller.
  - The order information is stored in the runtime's storage for both payer and seller references.
  - An event is emitted to signify that the order has been successfully placed.

