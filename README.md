# NFT-Art-Marketplace-
Create a platform to buy, sell, and auction digital art as NFTs.
module NFTArtMarketplace::Marketplace {

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
