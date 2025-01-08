// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721Enumerable.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721Burnable.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721Pausable.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/security/ReentrancyGuard.sol";
import "@openzeppelin/contracts/utils/Counters.sol";

contract K3LCollectible is 
    ERC721, 
    ERC721Enumerable, 
    ERC721URIStorage, 
    ERC721Burnable, 
    ERC721Pausable, 
    Ownable, 
    ReentrancyGuard 
{
    using Counters for Counters.Counter;
    Counters.Counter private _tokenIdCounter;

    enum Rarity { Common, Rare, Epic, Legendary }

    mapping(uint256 => Rarity) private _tokenRarity;
    mapping(uint256 => bool) private _isStaked;
    mapping(address => bool) public whitelist;

    uint256 public constant MAX_SUPPLY = 10000;
    uint256 public constant MAX_PER_WHITELIST = 2;
    uint256 public presalePrice = 0.05 ether;
    uint256 public publicPrice = 0.1 ether;
    bool public presaleActive = true;
    mapping(address => uint256) public whitelistMints;
    uint256 public marketplaceFee = 250; // 2.5%
    address public treasuryAddress;

    event TokenMinted(uint256 tokenId, address owner, string tokenURI, Rarity rarity);
    event TokenBurned(uint256 tokenId);
    event TokenStaked(uint256 tokenId, address owner);
    event TokenUnstaked(uint256 tokenId, address owner);
    event TokenURIUpdated(uint256 tokenId, string newURI);

    constructor(address _treasury) ERC721("K3L Collectible", "K3L") {
        treasuryAddress = _treasury;
    }

    function mint(address to, string memory tokenURI, Rarity rarity) public payable whenNotPaused nonReentrant {
        require(_tokenIdCounter.current() < MAX_SUPPLY, "Maximum supply reached");
        if (presaleActive) {
            require(whitelist[to], "Address not whitelisted");
            require(whitelistMints[to] < MAX_PER_WHITELIST, "Max whitelist mints reached");
            require(msg.value == presalePrice, "Incorrect presale price");
            whitelistMints[to]++;
        } else {
            require(msg.value == publicPrice, "Incorrect public sale price");
        }

        uint256 tokenId = _tokenIdCounter.current();
        _tokenIdCounter.increment();
        _safeMint(to, tokenId);
        _setTokenURI(tokenId, tokenURI);
        _tokenRarity[tokenId] = rarity;
        emit TokenMinted(tokenId, to, tokenURI, rarity);
    }

    function burn(uint256 tokenId) public override {
        require(ownerOf(tokenId) == msg.sender, "Only owner can burn this token");
        super.burn(tokenId);
        emit TokenBurned(tokenId);
    }

    function togglePresale() public onlyOwner {
        presaleActive = !presaleActive;
    }

    function setMarketplaceFee(uint256 _fee) public onlyOwner {
        require(_fee <= 1000, "Max fee is 10%");
        marketplaceFee = _fee;
    }

    function stakeNFT(uint256 tokenId) public whenNotPaused nonReentrant {
        require(ownerOf(tokenId) == msg.sender, "Only owner can stake this token");
        require(!_isStaked[tokenId], "Token is already staked");

        _isStaked[tokenId] = true;
        emit TokenStaked(tokenId, msg.sender);
    }

    function unstakeNFT(uint256 tokenId) public whenNotPaused nonReentrant {
        require(ownerOf(tokenId) == msg.sender, "Only owner can unstake this token");
        require(_isStaked[tokenId], "Token is not staked");

        _isStaked[tokenId] = false;
        emit TokenUnstaked(tokenId, msg.sender);
    }

    function calculateMarketplaceFee(uint256 salePrice) public view returns (uint256) {
        return (salePrice * marketplaceFee) / 10000;
    }

    function getTokenRarity(uint256 tokenId) public view returns (Rarity) {
        return _tokenRarity[tokenId];
    }

    function getVotingPower(uint256 tokenId) public view returns (uint256) {
        if (_tokenRarity[tokenId] == Rarity.Common) return 1;
        if (_tokenRarity[tokenId] == Rarity.Rare) return 3;
        if (_tokenRarity[tokenId] == Rarity.Epic) return 5;
        if (_tokenRarity[tokenId] == Rarity.Legendary) return 10;
        revert("Invalid token ID");
    }

    function updateTokenURI(uint256 tokenId, string memory newURI) public onlyOwner {
        _setTokenURI(tokenId, newURI);
        emit TokenURIUpdated(tokenId, newURI);
    }

    function addToWhitelist(address user) public onlyOwner {
        whitelist[user] = true;
    }

    function removeFromWhitelist(address user) public onlyOwner {
        whitelist[user] = false;
    }

    function pause() public onlyOwner {
        _pause();
    }

    function unpause() public onlyOwner {
        _unpause();
    }

    function tokensOfOwner(address owner) external view returns (uint256[] memory) {
        uint256 tokenCount = balanceOf(owner);
        uint256[] memory tokenIds = new uint256[](tokenCount);

        for (uint256 i = 0; i < tokenCount; i++) {
            tokenIds[i] = tokenOfOwnerByIndex(owner, i);
        }

        return tokenIds;
    }

    function _burn(uint256 tokenId) internal override(ERC721, ERC721URIStorage) {
        super._burn(tokenId);
    }

    function tokenURI(uint256 tokenId)
        public
        view
        override(ERC721, ERC721URIStorage)
        returns (string memory)
    {
        return super.tokenURI(tokenId);
    }

    function supportsInterface(bytes4 interfaceId)
        public
        view
        override(ERC721, ERC721Enumerable)
        returns (bool)
    {
        return super.supportsInterface(interfaceId);
    }

    function _beforeTokenTransfer(address from, address to, uint256 tokenId, uint256 batchSize)
        internal
        whenNotPaused
        override(ERC721, ERC721Enumerable, ERC721Pausable)
    {
        super._beforeTokenTransfer(from, to, tokenId, batchSize);
    }
}
