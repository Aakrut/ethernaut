# Challenge 0

<img src="./images/BigLevel0.svg" alt="0" >

Solution : Challenge 0
---
1. In this Challenge nothing but it will teach you how to interact with ABI.

2. read the instruction carefully espacially no. `6` and `9` it will help you pass this Challenge. First Get the new Instance and open your console tab whichever the browser you are using. 

3. First Interacting with abi. 
```js
await contract.abi
```
this command will exposes contracts ABI and you will see password at position 9.
```js
await contract.password()
```
it will give us the password as string. and you will see another authenticate it takes the passwordKey which in case the password you got earlier. Run this command
```js
await contract.authenticate("ethernaut0")
```
make sure to hit the submit instance button to see if this Challenge Successful. 🎉