# DegenToken

This Solidity program defines a smart contract for a token called "Degen Token" (symbol: DGN). It includes functionality for transferring tokens, minting new tokens, burning tokens, and managing items that can be redeemed using the tokens.

## Description

The DegenToken contract allows the contract owner to mint new tokens and burn existing ones. Users can transfer tokens to other addresses, and there is a system for adding items that can be redeemed with the tokens. Each item has an ID, name, and price. Users can redeem items if they have enough tokens.

## Getting Started

### Executing program

To run this program, you can use Remix, an online Solidity IDE. To get started, go to the Remix website at https://remix.ethereum.org/.

1. **Create a new file**: Click on the "+" icon in the left-hand sidebar and save the file with a `.sol` extension (e.g., `DegenToken.sol`).

2. **Copy and paste the code**: Copy the following Solidity code into the file:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.26;

contract DegenToken{
    
    address public  owner;
    uint public totalSupply;
    mapping (address => uint) public balanceOf;

    event Transfer(address indexed from, address indexed to, uint amount);

    struct Item {
        uint itemId;
        string itemName;
        uint itemPrice;
    }

    mapping(uint => Item) public items;
    uint public itemCount;

    constructor() {
        owner = msg.sender;
        totalSupply = 0;
    }

    modifier onlyOwner {
        require(msg.sender == owner, "You are not the Owner so you cant't Mint the Tokens");
        _;
    }

    string public constant name = "Degen Token";
    string public constant symbol = "DGN";

    function transfer(address _address, uint _value) external returns (bool) {
        require(balanceOf[msg.sender] >= _value, "You don't have enough Tokens to Transfer");

        balanceOf[msg.sender] -= _value;
        balanceOf[_address] += _value;

        emit Transfer(msg.sender, _address, _value);
        return true;
    }

    function mint(address _address,uint _value) external onlyOwner {
        balanceOf[_address] += _value;
        totalSupply += _value;
        emit Transfer(address(0), _address, _value);
    }

    function burn(uint _value) external {
        require(_value > 0, "You have Zero Tokens on the platform");
        require(balanceOf[msg.sender] >= _value, "You have less number of token then what you want to burn");
        balanceOf[msg.sender] -= _value;
        totalSupply -= _value;

        emit Transfer(msg.sender, address(0), _value);
    }
    
    function addItem(string memory _itemName, uint256 _itemPrice) external onlyOwner {
        itemCount++;
        Item memory newItem = Item(itemCount, _itemName, _itemPrice);
        items[itemCount] = newItem;
    }

    function getItems() external view returns (Item[] memory) {
        Item[] memory allItems = new Item[](itemCount);
        
        for (uint i = 1; i <= itemCount; i++) {
            allItems[i - 1] = items[i];
        }
        
        return allItems;
    }
    
    function redeem(uint _itemId) external {
        require(_itemId > 0 && _itemId <= itemCount, "Enterd Wrong number of Item!!");
        Item memory redeemedItem = items[_itemId];
        
        require(balanceOf[msg.sender] >= redeemedItem.itemPrice, "You can't Redeem this Item as you have less no of Tokens then the Required!!");
        
        balanceOf[msg.sender] -= redeemedItem.itemPrice;
        balanceOf[owner] += redeemedItem.itemPrice;
        emit Transfer(msg.sender, address(0), redeemedItem.itemPrice);
    }
}
```

3. **Compile the code**: Click on the "Solidity Compiler" tab in the left-hand sidebar. Make sure the "Compiler" option is set to "0.8.26" (or another compatible version), and then click on the "Compile DegenToken.sol" button.

4. **Deploy the contract**: Click on the "Deploy & Run Transactions" tab in the left-hand sidebar. Select the "DegenToken" contract from the dropdown menu, and then click on the "Deploy" button.

5. **Interact with the contract**: Once the contract is deployed, you can interact with it by calling its functions. For example, you can transfer tokens, mint new tokens (if you are the owner), burn tokens, add items, view items, and redeem items.

## Authors

Satyam Kumar
satyammjha0@gmail.com

## License

This project is licensed under the MIT License.
