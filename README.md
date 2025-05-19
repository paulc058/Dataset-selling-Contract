// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;


contract DatasetSale {


    address public owner;         // Owner of the contract
    string public datasetLink;    // IPFS link to the dataset
    uint256 public price;         // Price of the dataset (in wei)
    bool public datasetSold;      // Flag to check if the dataset has been sold


    // Event to emit when a dataset is sold
    event DatasetPurchased(address buyer, string datasetLink);


    // Constructor to initialize the contract
    constructor(string memory _datasetLink, uint256 _price) {
        owner = msg.sender;  // Set contract creator as the owner
        datasetLink = _datasetLink;  // IPFS link to the dataset
        price = _price;  // Price for the dataset in wei
        datasetSold = false;  // Set initial state as unsold
    }


    // Function to buy the dataset
    function buyDataset() public payable {
        require(!datasetSold, "Dataset has already been sold.");  // Ensure dataset hasn't been sold yet
        require(msg.value >= price, "Insufficient funds to purchase dataset."); // Ensure the buyer sends enough ETH


        // Transfer the payment to the contract owner
        payable(owner).transfer(msg.value);


        // Mark the dataset as sold
        datasetSold = true;


        // Emit an event to log the purchase
        emit DatasetPurchased(msg.sender, datasetLink);
    }


    // Function to check the contract balance (can be useful for the owner)
    function contractBalance() public view returns (uint256) {
        return address(this).balance;
    }


    // Function to withdraw funds for the owner (optional)
    function withdrawFunds() public {
        require(msg.sender == owner, "Only the owner can withdraw funds.");
        payable(owner).transfer(address(this).balance);
    }
}


