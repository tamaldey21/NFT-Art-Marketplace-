# NFT Art Marketplace

## Project Description
Create a platform to buy, sell, and auction digital art as NFTs.

## Project Vision
The NFT Art Marketplace aims to revolutionize the digital art industry by providing a decentralized platform for artists and collectors. By using blockchain technology, the platform ensures transparency, security, and trust in all transactions.

## Future Scope
- Expand support for multiple blockchain networks to facilitate cross-chain trading.
- Implement a rewards program for frequent buyers and sellers.
- Develop an AI-based recommendation system for personalized art suggestions.
- Introduce NFT fractional ownership to enable collective investments.
- Partner with galleries and museums for exclusive digital exhibitions.

## Key Features
- **NFT Minting**: Artists can create and mint NFTs for their digital artwork.
- **Buy and Sell**: Users can buy and sell NFTs with secure blockchain-based transactions.
- **Auctions**: Conduct and participate in live auctions for rare and exclusive NFTs.
- **Wallet Integration**: Compatible with major crypto wallets for seamless transactions.
- **Transparency**: Complete visibility into ownership history and transaction records through blockchain.

    use aptos_framework::signer;
    use aptos_framework::coin;
    use aptos_framework::aptos_coin::AptosCoin;

    struct NFT has store, key {
        owner: address,
        price: u64,
    }

    /// Function to list an NFT for sale.
    public fun list_nft(seller: &signer, price: u64) {
        let nft = NFT {
            owner: signer::address_of(seller),
            price,
        };
        move_to(seller, nft);
    }

    /// Function to buy an NFT.
    public fun buy_nft(buyer: &signer, seller_address: address) acquires NFT {
        let nft = borrow_global_mut<NFT>(seller_address);
        assert!(nft.owner == seller_address, 0);
        
        let payment = coin::withdraw<AptosCoin>(buyer, nft.price);
        coin::deposit<AptosCoin>(seller_address, payment);
        
        nft.owner = signer::address_of(buyer);
    }
}
Contract Address:
"0x4f7640138b6c111ebec758e31b821f76471cf03289761ac357ab8c106b970822"
