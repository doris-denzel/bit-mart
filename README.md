# BitMart Protocol

**Decentralized Bitcoin Commerce on Stacks Layer 2**

---

## 🧾 Overview

**BitMart** is a decentralized peer-to-peer commerce protocol built on the **Stacks blockchain**, enabling **trustless Bitcoin-native trade**. By leveraging **Stacks' smart contracts** and **Bitcoin's final settlement layer**, BitMart allows users to engage in direct sales or auctions with native BTC settlement, minimal fees, verified brands, and immutable transaction records—all without intermediaries.

---

## 🔐 System Architecture

BitMart combines the robustness of Bitcoin's **Proof-of-Work** security with Stacks' **Clarity smart contracts**, offering a secure, transparent, and decentralized protocol for commerce.

```
+------------------+      +-------------------+      +---------------------+
|  Buyers / Users  | <--> |  BitMart Protocol | <--> |  Merchants / Brands |
+------------------+      +-------------------+      +---------------------+
         |                        |                            |
         |                        V                            |
         |         +---------------------------+              |
         +-------> | Clarity Smart Contract(s) | <-------------+
                   +---------------------------+
                             |
                             V
                    +------------------+
                    |  Stacks Layer 2  |
                    +------------------+
                             |
                             V
                    +------------------+
                    |   Bitcoin L1     |
                    +------------------+
```

---

## 📦 Core Features

### 🛒 Commerce on Bitcoin

* Direct product listings or time-based auctions
* Payments settled in BTC via STX (Stacks token)
* Automated and transparent STX transfers

### 🏷️ Verified Brands

* Merchants register and optionally verify their brand
* Verification requires contract owner approval

### ⏱️ Auctions

* Reserve-price based auctions with time-bounded expiry
* Auto-refund of previous top bidder on outbidding
* Finalization only after block expiration

### 🧾 Reviews & Ratings

* Verified customers can leave reviews with 1-5 star ratings
* Prevents duplicate reviews per customer-product pair

### ⚖️ Minimal Fees

* Default platform fee of 2.5% (configurable by owner)
* All fee logic handled in protocol

---

## 📐 Contract Architecture

The protocol is implemented as a **single modular Clarity contract**.

### ✅ Constants & Error Codes

Defines reusable constants including error codes and platform-level configurations (e.g., `platform-fee-bps`).

### 🗂️ Data Maps

| Map Name   | Description                                              |
| ---------- | -------------------------------------------------------- |
| `brands`   | Merchant brand registry including verification status    |
| `products` | Product listings (both direct and auction items)         |
| `auctions` | Metadata for auction-based products                      |
| `reviews`  | Customer reviews, indexed by `product-id` and `customer` |

### 📥 Data Variables

* `product-counter`: Tracks the number of listed products
* `platform-fee-bps`: Basis point fee (e.g., 250 = 2.5%)

### 🔐 Access Control

* Only `CONTRACT_OWNER` can verify brands and update platform fees

---

## 🔄 Functional Breakdown

### 📁 Brand Management

* `register-brand`: Merchant registers a brand
* `verify-brand`: Admin verifies merchant identity

### 📦 Product Listings

* `list-product`: List product for fixed-price sale
* `buy-product`: Buyer purchases available product

### ⏱️ Auctions

* `create-auction`: Merchant lists a product as an auction
* `submit-bid`: Buyer places a bid, auto-refunding previous bidder
* `finalize-auction`: Transfers winning bid amount and closes auction

### ⭐ Reviews

* `submit-review`: Submit product review (1–5 stars)

### 🧾 Read-Only Queries

* `get-product-info`, `get-brand-info`, `get-auction-info`
* `get-product-review`, `get-platform-fee`, `get-product-count`

### ⚙️ Admin Functions

* `update-platform-fee`: Change platform fee (max 10%)

---

## 🔄 Optional Data Flow (Auction Example)

```text
Merchant → create-auction
        → Product listed w/ auction metadata
Buyer A → submit-bid (10 STX)
        → STX held in contract
Buyer B → submit-bid (15 STX)
        → Buyer A refunded, Buyer B becomes leader
After expiry → finalize-auction
        → STX sent to merchant (minus fee), product marked sold
```

---

## ✅ Security Considerations

* Uses `unwrap!`, `asserts!`, and `try!` for robust error and edge-case handling
* Prevents unauthorized actions through principal checks
* Platform fees are capped to prevent abuse
* Escrow logic for auctions avoids STX loss through refunds

---

## 🛠️ Deployment Considerations

* Set `CONTRACT_OWNER` correctly at deployment
* Verify that platform fee is set to a reasonable default
* Brand verification should be handled off-chain (e.g., KYC, manual review)

---

## 📜 License

MIT License – Open and extensible for any Bitcoin commerce use case.

---

## 🙋‍♂️ Contributing

We welcome contributions, reviews, and audits. Please fork the repository and submit a pull request with your proposed changes.
