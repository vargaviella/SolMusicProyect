# SolMusicProyect

The idea is to make connections from web3 to web2 so that you can interact with the functions of this contract that will soon be deployed, with the website. So that users can either mint their song, or they can buy a share of that song with mintV2(). They can also list their tokens, make offers, pay to play a song on their station, etc.

Solmusic integration from web2 to web3:

Create a web3 connection for external wallets (ex: Metamask).
Develop the platform to create the NFTs with the following steps: to. That the creator can upload music files with a cover. That the file is uploaded to the cloud and returns a unique hash of the file location. Complete with the name of the artist and name of the song to generate the metadata so that the file is uploaded to the cloud and returns a unique hash of the file location which serves to complete the baseUri of the NFT. That is, the baseUri = location of the metadata. And the metadata = location hash of the image stored in the cloud + artist name and song name. b. Integration of all smart-contract functions in .js to be able to interact from the website with the functions of the web3.
Constants:

uint256 public constant OWNER_ROYALTY_PERCENTAGE = 60; Percentage of royalties given to the original owner of the token. uint256 public constant CONTRACT_ROYALTY_PERCENTAGE = 10; Percentage of royalties given to the contract owner.

State Variables:

uint256 private _tokenCounter; Tracks the total number of minted tokens.

Mappings

Various mappings are used to store metadata and manage the contract's logic:

_baseUri: Stores the base URI for each token.

_artist: Stores the artist's name for each token.

_nameSong: Stores the song name for each token.

_initialPriceToMintAClone: Stores the initial price to mint a clone of each token.

_currentPriceToMintAClone: Stores the current price to mint a clone of each token.

_mintV1ParamsHash: Stores the hash of the minting parameters.

_tokenIdByMintV1ParamsHash: Maps the minting parameters hash to the token ID.

_balances: Stores the balance of each address for each token.

_usersForMintV1ParamsHash: Stores users who have interacted with a specific mintV1ParamsHash.

_exists: Tracks the existence of each token.

_tokenPrices: Stores the listing prices of tokens.

_tokenOffers: Stores offers made on tokens.

_offerCounter: Tracks the number of offers made on each token.

_hashContributions: Tracks contributions by users for each hash.

_totalHashContributions: Tracks total contributions for each hash.

_contributorsByHash: Stores contributors for each hash.

userCalledViewToken: Tracks if a user has called the viewToken function for a specific token.

reproductionEndTime: Stores the reproduction end time for a user for a specific token.

Structs

struct Offer { uint256 offerId; address offeror; uint256 amount; } Represents an offer made on a token.

Functions:

function mintV1( string memory baseUri, string memory artist, string memory nameSong, uint256 priceToMintAClone ) external nonReentrant Allows a user to mint a new music NFT with specific parameters. It increments the token counter, mints the token, and sets its metadata.

function mintV2(bytes32 mintV1ParamsHash) public payable nonReentrant Allows a user to mint a clone of an existing music NFT by providing a hash of the minting parameters. It verifies the hash, adjusts the minting price, and mints the clone token. The value sent is distributed as royalties.

function _adjustPriceToMintAClone(uint256 tokenId) private returns (uint256) Adjusts the price to mint a clone of a token based on the number of times the hash has been called.

function getTokenParamsHash(uint256 tokenId) public view returns (bytes32) Returns the minting parameters hash for a given token ID.

function getHashContribution(bytes32 mintV1ParamsHash) public view returns (uint256 totalBalance, address[] memory contributors) Returns the total balance and contributors for a specific minting parameters hash.

function withdrawRoyalties(bytes32 mintV1ParamsHash) public nonReentrant Allows users to withdraw their share of royalties based on their contributions.

function getBaseUri(uint256 tokenId) external view returns (string memory) Returns the base URI for a given token ID.

function getArtist(uint256 tokenId) external view returns (string memory) Returns the artist's name for a given token ID.

function getNameSong(uint256 tokenId) external view returns (string memory) Returns the song name for a given token ID.

function getPriceToMintAClone(bytes32 mintV1ParamsHash) external view returns (uint256) Returns the current price to mint a clone of a token based on the minting parameters hash.

function _generateMintV1ParamsHash( string memory baseUri, string memory artist, string memory nameSong, uint256 priceToMintAClone ) private pure returns (bytes32) Generates a unique hash for the minting parameters.

function _findTokenId(bytes32 mintV1ParamsHash) private view returns (uint256) Finds the token ID based on the minting parameters hash.

function tokenURI(uint256 tokenId) public view virtual override returns (string memory) Returns the URI for a given token ID.

function listForSale(uint256 tokenId, uint256 price) external Allows the owner of a token to list it for sale at a specified price.

function makeOffer(uint256 tokenId) public payable Allows a user to make an offer on a token that is listed for sale.

function withdrawOffer(uint256 tokenId, uint256 offerId) public nonReentrant Allows a user to withdraw their offer on a token.

function acceptOffer(uint256 tokenId, uint256 offerId) external nonReentrant Allows the owner of a token to accept an offer, transferring the token to the buyer and distributing the payment as royalties.

function viewToken(uint256 tokenId) external payable nonReentrant Allows a user to view the token by paying a fee. The fee is distributed as royalties.

function canReproduce(uint256 tokenId, address user) external view returns (bool) Checks if a user can reproduce the token based on whether they have called the viewToken function within the last hour.

function getOfferDetails(uint256 tokenId) public view returns (Offer[] memory) Returns all offers made on a given token.
