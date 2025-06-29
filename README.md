# 🏆 NFT Marketplace on Aptos Blockchain — 🤑 Won $60 Bounty!
---

## 📸 Screenshots
 ![Screenshot 2](./Screenshot%202025-06-29%20143308.png) 
 ![Screenshot 1](./Screenshot%202025-06-29%20143213.png)

---





# NFT Marketplace on Aptos Blockchain

Welcome to **NFT Marketplace**, a feature-rich platform built on the **Aptos Blockchain** that allows users to **create, bid, transfer**, and **trade NFTs** securely. The marketplace integrates various powerful features like auctions, royalties, offers, and advanced filtering, all powered by **Aptos Move** smart contracts. The marketplace also facilitates payments in **APT tokens** and supports tipping for creators.

---

## 🚀 Key Features

### **1. Auction System**
- Create, bid, and win NFTs via auctions.
- Sellers can set the starting price and auction duration.
- Bidders can place incremental bids until the auction ends, with the highest bidder winning.

### **2. Royalties and Secondary Sales**
- Ensure creators earn royalties from secondary sales.
- Every time an NFT is resold, a predefined percentage of the sale price goes directly to the creator.

### **3. NFT Transfers**
- Users can transfer NFTs between wallets directly within the marketplace.
- Simple and secure transfer system for gifting or trading NFTs.

### **4. Offer System**
- Buyers can make offers on listed NFTs.
- Sellers have the ability to accept, decline, or ignore offers based on their preferences.

### **5. Advanced Filtering and Sorting**
- Filter NFTs by:
  - **Rarity**
  - **Price Range**
  - **Date Listed**
- Sort NFTs by:
  - **Highest Price**
  - **Lowest Price**
  - **Newest Listings**
- Improve user experience by easily finding desired NFTs.

### **6. APT Token Integration**
- Facilitates payments in **APT tokens** for all marketplace transactions.
- Introduced a **tipping** feature, allowing users to send donations to NFT creators as a form of appreciation.

---

## 🔗 Links

- **Smart Contract Address**:  
  [View Smart Contract on Aptos Explorer](https://explorer.aptoslabs.com/account/0x3ed23f75dc96ed785388d48d31252e98e3b031fb3cdca6175f0a9c75d4489521/modules/code/NFTMarketplace?network=testnet)

- **Demo Video**:  
  [Watch the Demo](https://youtu.be/ktyZZIuZpNY?feature=shared)

---

## 🛠️ Setup Instructions

### **Backend Setup: Deploying the Aptos Smart Contract**

1. **Install the Aptos CLI**:
   ```bash
   curl -fsSL "https://aptos.dev/scripts/install_cli.py" | python3
   ```

2. **Initialize Aptos Project**:
   ```bash
   aptos init
   ```

3. **Compile the Smart Contract**:
   ```bash
   aptos move compile
   ```

4. **Publish the Contract**:
   ```bash
   aptos move publish
   ```

### **Frontend Setup: React Application**

1. Navigate to the frontend directory:
   ```bash
   cd aptos4-q1
   ```

2. **Install Dependencies**:
   ```bash
   npm install
   npm i @aptos-labs/wallet-adapter-react @aptos-labs/wallet-adapter-ant-design petra-plugin-wallet-adapter --legacy-peer-deps
   npm install react-router-dom --legacy-peer-deps
   ```

3. **Start the Development Server**:
   ```bash
   npm start
   ```

---

## 📜 Contract Details

In the frontend code, replace `"YOUR_MARKETPLACE_ADDRESS_HERE"` with the following smart contract address:

**`0x3ed23f75dc96ed785388d48d31252e98e3b031fb3cdca6175f0a9c75d4489521`**

---

## 📖 Features Explained

### **Auction System**
- **Create an Auction**: Sellers can list NFTs with a custom starting price and duration for bidding.
- **Place Bids**: Buyers can place bids that increase incrementally. The highest bidder wins the NFT after the auction ends.
- **End Auction**: The auction concludes by transferring the NFT to the highest bidder and releasing the payment (minus fees) to the seller.

### **Royalties for Creators**
- Each time an NFT is resold, a percentage of the sale price is sent directly to the original creator, ensuring that creators continue to benefit from their work.

### **NFT Transfers**
- Easily transfer NFTs to any wallet address directly through the marketplace.
- Batch transfer allows users to send multiple NFTs in one transaction.

### **Offer System**
- **Buyers** can propose offers on NFTs listed in the marketplace.
- **Sellers** can choose to accept, decline, or ignore incoming offers.

### **APT Token Payments**
- Use **APT tokens** for all transactions, including NFT purchases and sales.
- Introduced a **tip/donation** feature for users to send appreciation to creators directly.

### **Advanced Filters and Sorting**
- **Filter NFTs** by various criteria such as **rarity**, **price range**, and **date listed**.
- **Sort NFTs** by price (highest/lowest) and listing date to find the best deals quickly.

---

Here’s the complete content for your **README** file, incorporating the auction system, royalties, and all other features:

---

## **Marketplace Features and Backend Code Explanation**

This document provides an overview of the core features implemented in the marketplace, including the auction system, royalty management, NFT transfers, and other key functionalities. The backend code is implemented in **Move**, the programming language for the Aptos blockchain.

---

### **1. Auction System**
**Feature:**
- **Create, bid, and win NFTs via auctions.**
- Sellers can set the starting price and auction duration.
- Bidders can place incremental bids until the auction ends, with the highest bidder winning.

**Backend Code Block:**

```rust
// Auction System
struct Auction has store {
    nft_id: u64,
    seller: address,
    start_price: u64,
    current_price: u64,
    highest_bidder: address,
    start_time: u64,
    end_time: u64,
    active: bool
}

// Create new auction
public entry fun create_auction(
    account: &signer,
    marketplace_addr: address,
    nft_id: u64,
    start_price: u64,
    duration: u64
) acquires AuctionStore, Marketplace { ... }

// Place bid
public entry fun place_bid(
    account: &signer,
    marketplace_addr: address,
    auction_id: u64,
    bid_amount: u64
) acquires AuctionStore { ... }

// End auction
public entry fun end_auction(
    account: &signer,
    marketplace_addr: address,
    auction_id: u64
) acquires AuctionStore, Marketplace { ... }
```

**Explanation:**
- **create_auction:** Allows sellers to create an auction for an NFT, setting the starting price and auction duration.
- **place_bid:** Allows bidders to place bids on active auctions.
- **end_auction:** Ends the auction, transferring the NFT to the highest bidder and distributing funds to the seller after deducting marketplace fees.

---

### **2. Royalties and Secondary Sales**
**Feature:**
- **Ensure creators earn royalties from secondary sales.**
- A predefined percentage of the sale price goes directly to the creator on each resale.

**Backend Code Block:**

```rust
struct NFT has store, key {
    id: u64,
    owner: address,
    name: vector<u8>,
    description: vector<u8>,
    uri: vector<u8>,
    price: u64,
    for_sale: bool,
    rarity: u8, // 1 for common, 2 for rare, 3 for epic, etc.
    listing_date: u64,
    creator: address,
    royalty_percentage: u64,
}

const DEFAULT_ROYALTY_PERCENTAGE: u64 = 5; // 5% royalty
const MAX_ROYALTY_PERCENTAGE: u64 = 15; // 15% maximum royalty

// Define Marketplace Structure
struct Marketplace has key {
    nfts: vector<NFT>
}

// Define ListedNFT Structure
struct ListedNFT has copy, drop {
    id: u64,
    price: u64,
    rarity: u8
}
```

**Explanation:**
- **NFT Struct:** The `NFT` struct stores information about each NFT, including the owner, price, rarity, and creator, along with the creator’s royalty percentage.
- **DEFAULT_ROYALTY_PERCENTAGE:** Default royalty set to 5% for creators, but this can be adjusted within the allowable range of 5% to 15%.
- The royalty percentage is applied during transactions like purchases or auctions to ensure that creators earn a fair share of the proceeds from secondary sales.

---

### **3. NFT Transfers**
**Feature:**
- **Users can transfer NFTs between wallets directly within the marketplace.**
- Simple and secure transfer system for gifting or trading NFTs.

**Backend Code Block:**

```rust
public entry fun transfer_nft(
    account: &signer,
    marketplace_addr: address,
    nft_id: u64,
    recipient: address
) acquires Marketplace { ... }

// Batch transfer function
public entry fun batch_transfer_nfts(
    account: &signer,
    marketplace_addr: address,
    nft_ids: vector<u64>,
    recipient: address
) acquires Marketplace { ... }
```

**Explanation:**
- **transfer_nft:** Transfers an NFT from one wallet to another.
- **batch_transfer_nfts:** Allows multiple NFTs to be transferred in one transaction, reducing overhead for users with large collections.

---

### **4. Offer System**
**Feature:**
- **Buyers can make offers on listed NFTs.**
- Sellers have the ability to accept, decline, or ignore offers based on their preferences.

**Backend Code Block:**

```rust
struct Offer has store {
    buyer: address,
    nft_id: u64,
    amount: u64,
    timestamp: u64,
    status: u8, // 0: pending, 1: accepted, 2: declined
    index: u64
}

public entry fun make_offer(
    account: &signer,
    marketplace_addr: address,
    nft_id: u64,
    offer_amount: u64
) acquires OfferStore, Marketplace { ... }

public entry fun accept_offer(
    account: &signer,
    marketplace_addr: address,
    offer_index: u64
) acquires OfferStore, Marketplace { ... }

public entry fun decline_offer(
    account: &signer,
    marketplace_addr: address,
    offer_index: u64
) acquires OfferStore, Marketplace { ... }
```

**Explanation:**
- **make_offer:** Allows buyers to make offers for listed NFTs.
- **accept_offer:** Sellers can accept an offer, transferring the NFT to the buyer and the payment to the seller.
- **decline_offer:** Sellers can reject offers, preventing the transfer of the NFT.

---

### **5. Advanced Filtering and Sorting**
**Feature:**
- **Filter NFTs by:**
  - Rarity
  - Price Range
  - Date Listed
- **Sort NFTs by:**
  - Highest Price
  - Lowest Price
  - Newest Listings

**Backend Code Block:**
```rust
// Advanced filtering and sorting would likely be implemented in the frontend.
```

**Explanation:**
- The backend can expose filtering and sorting endpoints, allowing the frontend to query NFTs by various criteria such as rarity, price, and date listed. This enables users to easily find the NFTs they are interested in.

---

### **6. APT Token Integration**
**Feature:**
- **Facilitates payments in APT tokens for all marketplace transactions.**
- Introduced a tipping feature, allowing users to send donations to NFT creators as a form of appreciation.

**Backend Code Block:**

```rust
// transfer_apt and batch_transfer_apt for transferring APT coins
public entry fun transfer_apt(
    account: &signer,
    recipient: address,
    amount: u64
) { ... }

public entry fun batch_transfer_apt(
    account: &signer,
    recipients: vector<address>,
    amounts: vector<u64>
) { ... }

public entry fun purchase_nft_with_tip(
    account: &signer,
    marketplace_addr: address,
    nft_id: u64,
    payment: u64,
    tip_amount: u64
) acquires Marketplace { ... }
```

**Explanation:**
- **transfer_apt:** Enables the transfer of APT tokens for payments and transactions within the marketplace.
- **batch_transfer_apt:** Facilitates transferring APT tokens to multiple recipients at once.
- **purchase_nft_with_tip:** Allows users to buy NFTs using APT tokens and optionally tip the creator, adding a layer of support for creators.

---

