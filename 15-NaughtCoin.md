# Challenge 15

<img src="./images/BigLevel15.svg" alt="15" >

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import 'openzeppelin-contracts-08/token/ERC20/ERC20.sol';

 contract NaughtCoin is ERC20 {

  // string public constant name = 'NaughtCoin';
  // string public constant symbol = '0x0';
  // uint public constant decimals = 18;
  uint public timeLock = block.timestamp + 10 * 365 days;
  uint256 public INITIAL_SUPPLY;
  address public player;

  constructor(address _player) 
  ERC20('NaughtCoin', '0x0') {
    player = _player;
    INITIAL_SUPPLY = 1000000 * (10**uint256(decimals()));
    // _totalSupply = INITIAL_SUPPLY;
    // _balances[player] = INITIAL_SUPPLY;
    _mint(player, INITIAL_SUPPLY);
    emit Transfer(address(0), player, INITIAL_SUPPLY);
  }
  
  function transfer(address _to, uint256 _value) override public lockTokens returns(bool) {
    super.transfer(_to, _value);
  }

  // Prevent the initial owner from transferring tokens until the timelock has passed
  modifier lockTokens() {
    if (msg.sender == player) {
      require(block.timestamp > timeLock);
      _;
    } else {
     _;
    }
  } 
} 
```

Challenge
---
> NaughtCoin is an ERC20 token and you're already holding all of them. The catch is that you'll only be able to transfer them after a 10 year lockout period. Can you figure out how to get them out to another address so that you can transfer them freely? Complete this level by getting your token balance to 0.

Solution 
---
1. If You Look at the ERC20 Contract There is `transferFrom` function, but first we need approval so let's look at the `approve` function 

```solidity
    function _approve(address owner, address spender, uint256 amount) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
```

it says owner cannot be 0 and spender also so we can do this 
First Check See Our balance.
```js
(await contract.balanceOf(player)).toString()
// '1000000000000000000000000'
```

then let's Appprove for ourself to transfer
```js
await contract.approve(player, "1000000000000000000000000");
```

then let's see there is function call `allowance` how many amount spender allowed to spend. in our case we are the owner and spender also.
```js
(await contract.allowance(player, player)).toString();
// '1000000000000000000000000'
```

now we can transfer easily to another address using `transferFrom` function
```js
await contract.transferFrom(player,OTHER_ADDRESS,"1000000000000000000000000")
```
you will Successfully send amount to the `OTHER_ADDRESS` and for checking if amount is deducted or not.

```js
(await contract.balanceOf(player)).toString();
// "0"
```

You now completed Challenge, now submit the instance your challenge will be cleared.🎉