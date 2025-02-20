// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract AIGeneratedArtNFT {
    string public name = "AI Art NFT";
    string public symbol = "AINFT";
    uint256 public nextTokenId;
    address public owner;

    struct NFT {
        address owner;
        string metadata;
    }

    mapping(uint256 => NFT) public tokens;
    mapping(address => uint256[]) public ownerTokens;

    event Minted(address indexed to, uint256 tokenId, string metadata);
    event MetadataUpdated(uint256 indexed tokenId, string newMetadata);

    modifier onlyOwner() {
        require(msg.sender == owner, "Not the contract owner");
        _;
    }

    constructor() {
        owner = msg.sender;
    }

    function mintNFT(address to, string memory initialMetadata) public onlyOwner {
        uint256 tokenId = nextTokenId;
        tokens[tokenId] = NFT(to, initialMetadata);
        ownerTokens[to].push(tokenId);
        nextTokenId++;

        emit Minted(to, tokenId, initialMetadata);
    }

    function updateMetadata(uint256 tokenId, string memory newMetadata) public onlyOwner {
        require(tokens[tokenId].owner != address(0), "Token does not exist");
        tokens[tokenId].metadata = newMetadata;
        emit MetadataUpdated(tokenId, newMetadata);
    }

    function getMetadata(uint256 tokenId) public view returns (string memory) {
        require(tokens[tokenId].owner != address(0), "Token does not exist");
        return tokens[tokenId].metadata;
    }
}
