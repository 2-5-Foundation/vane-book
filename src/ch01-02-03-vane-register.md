# Vane-Register

## Introduction

The module defines callabale methods that facilitates the registration of both payers and payees in a decentralized system. Users can register as either payers or payees, and additional functionalities include product registration by payees. This module interacts with various other Substrate pallets and provides custom storage, events, errors, and callable functions.
This module can be integrated into a Substrate runtime to enable user registration and product management functionalities in a decentralized system. Users can register as payers or payees, and payees can register and update their product offerings. The module also emits events to track user registration.

## Configuration

The behavior of this module can be customized through a configuration trait `Config`. The configuration trait specifies the runtime's requirements and includes the following traits:

- `frame_system::Config`: Required for system-related functionality.
- `Currency<Self::AccountId>`: The currency type used for transactions in the runtime.
- `RuntimeEvent`: Events generated by this module.

## Storage

1. **PayeeStorage**:
   - **Description**: This storage item is an unbounded map that associates a payee's account with a `PayeeAccountProfile<T>`. Each `PayeeAccountProfile` contains information about the payee, including name, location, Instagram link, and registration time.
   - **Storage Type**: `StorageMap<_, Blake2_128, T::AccountId, PayeeAccountProfile<T>>`

2. **PayerStorage**:
   - **Description**: This storage item is an unbounded map that associates a payer's account with a `PayerAccountProfile<T>`. Each `PayerAccountProfile` contains information about the payer, including name, KYC (Know Your Customer) information, email, Vane ID, and registration time.
   - **Storage Type**: `StorageMap<_, Blake2_128, T::AccountId, PayerAccountProfile<T>>`

3. **PayeeProducts**:
   - **Description**: This storage item is an unbounded map that associates a payee's account with a vector of `ProductProfile<T>`. Each `ProductProfile` represents a product registered by the payee and contains details such as product ID, image URL, price, and seller's account.
   - **Storage Type**: `StorageMap<_, Blake2_128Concat, T::AccountId, Vec<ProductProfile<T>>, ValueQuery>`

## Events

1. **PayerRegistered**:
   - **Description**: This event is emitted when a payer is successfully registered. It includes the payer's account ID and the registration time.
   - **Event Fields**:
     - `id`: The account ID of the registered payer.
     - `time`: The block number indicating the registration time.

2. **PayeeRegistered**:
   - **Description**: This event is emitted when a payee is successfully registered. It includes the payee's account ID and the registration time.
   - **Event Fields**:
     - `id`: The account ID of the registered payee.
     - `time`: The block number indicating the registration time.

## Errors

1. **AccountAlreadyRegistered**:
   - **Description**: An error indicating that the account being registered (payer or payee) is already registered in the system.
2. **UserIsNotRegistered**:
   - **Description**: An error indicating that the user attempting to perform an operation is not registered in the system.

## Callable Functions

### `register_payer`

- **Parameters**:
  - `origin`: An `OriginFor<T>` representing the caller's origin.
  - `name`: An optional `Vec<u8>` representing the payer's name.
  - `email`: An optional `Vec<u8>` representing the payer's email.
  - `kyc`: An optional `Vec<u8>` representing KYC (Know Your Customer) information.

- **Description**:
  - This function allows a user to register as a payer in the system. The caller's identity is verified, and if the payer is not already registered, the registration is completed.
  - The payer's Vane ID is generated from a hash of their KYC information.
  - Payer account information is stored in the runtime's storage.
  - An event is emitted to signify that the payer has been successfully registered.

### `register_payee`

- **Parameters**:
  - `origin`: An `OriginFor<T>` representing the caller's origin.
  - `name`: A `Vec<u8>` representing the payee's name.
  - `ig_link`: A `Vec<u8>` representing the payee's Instagram link.
  - `location`: A `Vec<u8>` representing the payee's location.

- **Description**:
  - This function allows a user to register as a payee in the system. The caller's identity is verified, and if the payee is not already registered, the registration is completed.
  - Payee account information is stored in the runtime's storage.
  - An event is emitted to signify that the payee has been successfully registered.

### `update_products`

- **Parameters**:
  - `origin`: An `OriginFor<T>` representing the caller's origin.
  - `product_id`: A `u32` representing the ID of the product.
  - `link`: A `Vec<u8>` representing the product's URL link.
  - `amount`: The price of the product.
  - `image_url`: An optional `Vec<u8>` representing the product's image URL.

- **Description**:
  - This function allows a registered payee to update and add products to their profile.
  - The caller's identity is verified, and the system checks if the payee is registered.
  - A new `ProductProfile` is created with the provided product information and added to the payee's list of products.
  - Product information includes the product's ID, image URL, price, and seller's account.
  - The added product is stored in the runtime's storage.
  - This function enables payees to manage their product offerings.



