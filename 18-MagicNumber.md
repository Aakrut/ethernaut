# Challenge 18

<img src="./images/BigLevel18.svg" alt="18" >

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
contract MagicNum {

  address public solver;

  constructor() {}

  function setSolver(address _solver) public {
    solver = _solver;
  }

  /*
    ____________/\\\_______/\\\\\\\\\_____        
     __________/\\\\\_____/\\\///////\\\___       
      ________/\\\/\\\____\///______\//\\\__      
       ______/\\\/\/\\\______________/\\\/___     
        ____/\\\/__\/\\\___________/\\\//_____    
         __/\\\\\\\\\\\\\\\\_____/\\\//________   
          _\///////////\\\//____/\\\/___________  
           ___________\/\\\_____/\\\\\\\\\\\\\\\_ 
            ___________\///_____\///////////////__
  */
}
```

Challenge
---
> To solve this level, you only need to provide the Ethernaut with a Solver, a contract that responds to whatIsTheMeaningOfLife() with the right number.

Easy right? Well... there's a catch.

The solver's code needs to be really tiny. Really reaaaaaallly tiny. Like freakin' really really itty-bitty tiny: 10 opcodes at most.

Hint: Perhaps its time to leave the comfort of the Solidity compiler momentarily, and build this one by hand O_o. That's right: Raw EVM bytecode.

Solution
---
Here's The Link [OpenZeppelin EVM Series](https://blog.openzeppelin.com/deconstructing-a-solidity-contract-part-i-introduction-832efd2d7737/).
---
```js
await web3.eth.sendTransaction({from:player,data:"0x600a600c600039600a6000f3604260805260206080f3"})
```

go to the whatever network you are using for game and check the latest tx you will see  contract creation get that address and paste into the setSolver function as argument.

```shell
> await contract.setSolver(CONTRACT_CREATION_ADDRESS);
```
 now submit the instance your challenge will be cleared.🎉