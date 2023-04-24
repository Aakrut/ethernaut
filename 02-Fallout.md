# Challenge 2

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

import 'openzeppelin-contracts-06/math/SafeMath.sol';

contract Fallout {
  
  using SafeMath for uint256;
  mapping (address => uint) allocations;
  address payable public owner;


  /* constructor */
  function Fal1out() public payable {
    owner = msg.sender;
    allocations[owner] = msg.value;
  }

  modifier onlyOwner {
	        require(
	            msg.sender == owner,
	            "caller is not the owner"
	        );
	        _;
	    }

  function allocate() public payable {
    allocations[msg.sender] = allocations[msg.sender].add(msg.value);
  }

  function sendAllocation(address payable allocator) public {
    require(allocations[allocator] > 0);
    allocator.transfer(allocations[allocator]);
  }

  function collectAllocations() public onlyOwner {
    msg.sender.transfer(address(this).balance);
  }

  function allocatorBalance(address allocator) public view returns (uint) {
    return allocations[allocator];
  }
}
```
Challenge
---
> Claim ownership of the contract below to complete this level.

Solution : Challenge 2
---

1. We can claim the ownership only by constructor, but if you look at the contract then you will the `Fal1out()` is not the same as `Fallout`. (ex Solidity Version 0.6.0 and that means constructor name should be same as the contract name.) which in case our contract name is `Fallout` but our constructor is `Fal1out` that means `Fal1out` becomes the function that anyone can call.

2. get the new instance and call Fal1out() function 
```js
await contract.Fal1out()
```
and then submit your instance you will pass the Challenge.