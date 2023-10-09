# Vane-XCM


## Pallet Calls

Certainly, let's go through the callable methods defined in the provided Rust code for the `Pallet` module in detail:

1. **`vane_transfer`**:
   - **Description**:
     - This function is responsible for initiating a transfer from the caller's account to the specified recipient account.
     - It first ensures that the caller has the proper authorization (through `ensure_signed`).
     - Depending on the `currency` type currently we have `Token::Dot` and in future we will be supporting `Token::Usdt`, it performs specific actions.
     - If `currency` is `Token::Dot`, it initiates an XCM (Cross-Chain Message) transfer.

2. **`vane_confirm`**:
   - **Description**:
     - This function is used for confirming a transfer initiated by another party.
     - It checks the caller's authorization using `ensure_signed`.
     - If the caller is the payer, it checks if the payee has already confirmed. If not, it adds the payer to the list of confirmed signers and performs various operations.
     - Once the caller is the validated as payee, it adds the payee to the list of confirmed signers.
     - It derives multi-identifiers for confirmed and allowed signers, ensuring they match.
     - If the multi-identifiers match, it dispatches an XCM call for confirmation.

3. **`test_storing`**:
   - **Description**:
     - This function allows the caller to store a number associated with a specific account in a storage map.
     - It ensures that the caller is authorized using `ensure_signed`.
     - It stores the provided `num` associated with the provided `acc` in the `TestStorage` storage map.
     - It emits an `Event::TestStored` event to signal that the storing operation was successful.

These callable methods are part of the `Pallet` and provide functionality related to multi-signature transfers, confirmation of transfers, and a simple storage mechanism for testing purposes. Depending on the specific use case and configuration of your Substrate runtime, these methods may have different behavior or additional requirements.