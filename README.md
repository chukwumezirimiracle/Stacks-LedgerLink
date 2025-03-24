# STX-LedgerLink - Decentralized Trading Platform

## Overview
STX-LedgerLink is a decentralized trading platform built on the Stacks blockchain using the Clarity smart contract language. It facilitates secure and transparent peer-to-peer trading of digital assets. Users can create listings for assets, make offers, and manage transactions seamlessly. The platform includes features such as asset listings, offers management, refunds for canceled offers, and controlled listing expiry.

---

## Features

- **Create Listings:** Users can list digital assets with a specified price and expiry block.
- **Make and Cancel Offers:** Buyers can make offers on listed assets and cancel pending offers.
- **Refund Mechanism:** Ensures buyers receive their funds back if an offer is canceled.
- **Listing Expiry:** Automatically handles expired listings.
- **Robust Error Handling:** Provides detailed error codes for various failure conditions.

---

## Constants

| Constant              | Description                                  | Value      |
|------------------------|----------------------------------------------|-----------|
| `contract-owner`       | The address of the contract deployer.         | `tx-sender` |
| `err-not-authorized`   | Error when a user is not authorized.          | `(err u100)` |
| `err-listing-not-found`| Error when a listing is not found.            | `(err u101)` |
| `err-invalid-status`   | Error when the status is invalid.             | `(err u102)` |
| `err-insufficient-balance`| Error for insufficient balance.             | `(err u103)` |
| `err-no-active-offer`  | Error when no active offer exists.            | `(err u104)` |
| `err-listing-expired`  | Error when a listing has expired.             | `(err u105)` |

---

## Data Structures

### 1. **Listings Map**
```clarity
(define-map listings uint {
    seller: principal,
    asset: (string-ascii 32),
    price: uint,
    status: (string-ascii 10),
    expiry: uint
})
```
- Stores each listing with seller details, asset information, price, status, and expiry block.

### 2. **Offers Map**
```clarity
(define-map offers {listing-id: uint, buyer: principal} {
    amount: uint,
    status: (string-ascii 10)
})
```
- Tracks offers made by buyers, including amount and current status.

### 3. **Listing Nonce**
```clarity
(define-data-var listing-nonce uint u0)
```
- A counter to generate unique listing IDs.

---

## Functions

### 1. **Create Listing**
```clarity
(define-public (create-listing (asset (string-ascii 32)) (price uint) (expiry uint))
```
- Creates a new asset listing with a specified expiry block.
- **Parameters:**
  - `asset`: Asset description.
  - `price`: Price of the asset.
  - `expiry`: Block height when the listing expires.
- **Returns:** Unique `listing-id`.

---

### 2. **Cancel Listing**
```clarity
(define-public (cancel-listing (listing-id uint))
```
- Cancels an active listing if the caller is the seller.
- **Parameters:**
  - `listing-id`: ID of the listing to be canceled.
- **Returns:** `true` if successful.

---

### 3. **Cancel Offer**
```clarity
(define-public (cancel-offer (listing-id uint))
```
- Cancels a pending offer made by the caller.
- **Parameters:**
  - `listing-id`: ID of the listing the offer is associated with.
- **Returns:** `true` if the offer was successfully canceled.

---

### 4. **Refund Active Offer** *(Private)*
```clarity
(define-private (refund-active-offer (listing-id uint) (buyer principal))
```
- Refunds the buyer if they have a pending offer.
- **Parameters:**
  - `listing-id`: ID of the listing.
  - `buyer`: Principal of the buyer.
- **Returns:** `true` if the refund was successful.

---

## Error Handling

The contract uses specific error codes to identify and handle exceptions gracefully. For example:

- **u100:** Not authorized to perform the action.
- **u101:** Listing not found.
- **u102:** Invalid status.
- **u103:** Insufficient balance.
- **u104:** No active offer found.
- **u105:** Listing has expired.

---

## How to Deploy

1. Clone the repository or download the contract file.
2. Deploy the contract using Clarinet or the Stacks Explorer.
3. Ensure the necessary variables and constants are properly initialized.

---

## How to Interact

- **Create Listing:** Call `create-listing` with asset details.
- **Cancel Listing:** Call `cancel-listing` with the listing ID.
- **Make Offer:** *(To be implemented)*
- **Cancel Offer:** Call `cancel-offer` with the listing ID.

---

## Security Considerations

- Only the seller can cancel an active listing.
- Refunds are processed only for pending offers.
- The use of explicit error codes ensures predictable behavior.

---

## Future Improvements

- Add `make-offer` and `accept-offer` functions.
- Implement time-based automatic expiration.
- Add reputation tracking for buyers and sellers.

---

## Conclusion
STX-LedgerLink offers a robust foundation for decentralized asset trading with essential marketplace functionalities. The contract ensures secure handling of listings, offers, and refunds while maintaining transparency through immutable records on the blockchain.

