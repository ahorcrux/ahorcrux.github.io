---
title: Solidity 用例 | 批量转账
author: since2014
date: 2023-06-17 11:26:23 +0800
categories: [Technology, Blockchain]
tags: [blockchain, web3, solidity]
render_with_liquid: false
---

## 合约代码

### V1
```js
//SPDX-License-Identifier: MID
pragma solidity ^0.8.14;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

contract BatchTransferEth {
     
     address payable public owner;

     modifier ownable(){
          require(msg.sender == owner, "not owner");
          _;
     }

     constructor() payable {
          owner = payable(msg.sender);
     }

     function getBalance() public view returns(uint256){
          return address(this).balance;
     }

     //批量转ETH给所有地址，ETH数量平均分配，调用时指定的 value 值即总的 ETH 数量
     function transferAvgEthToAll(address[] calldata _toAddresses ) payable ownable public returns (bool){
          require(_toAddresses.length > 0, "toAddresses is empty");
          require(msg.value > 0, "the amount of eth for transfer is zero");
          uint avgEth = address(this).balance / _toAddresses.length; 
          for(uint32 i = 0; i < _toAddresses.length; i++){
               payable(_toAddresses[i]).transfer(avgEth);
          }
          return true;
     }

     //批量转ETH给所有地址，ETH 指定分配
     function transferDiffEthToAll(
          address payable[] calldata _toAddresses, 
          uint256[] calldata _values
          ) payable ownable public returns (bool){
          require(_toAddresses.length > 0, "toAddresses is empty");
          require(_toAddresses.length <= _values.length, "values invalid");
          uint256 totalAmount = getSum(_values);
          require(msg.value >= totalAmount, "eth not enough");
          for(uint32 i = 0; i < _toAddresses.length; i++){
               _toAddresses[i].transfer(_values[i]);
          }
          return true;
     }

     //给当前合约授权erc20 token转账的数量: 这种方式 授权的地址不是msg.sender ，而是合约地址
     // function approve(address _token, uint256 amount) ownable public returns(bool){
     //      IERC20 token = IERC20(_token);
     //      return token.approve(address(this), amount);
     // }

     // function getApprove(address _token) view public returns(uint256){
     //      IERC20 token = IERC20(_token);
     //      return token.allowance(msg.sender, address(this));
     // }

     // erc20 token 一对多批量转账，平均计算 token 数量
     function transferAvgERC20ToAll(
          address _token, 
          address payable[] calldata _toAddresses, 
          uint256 _amount
          ) ownable public returns(bool){
          require(_toAddresses.length > 0, "invalid receiver");
          require(_amount > 0, "invalid token amount");
          //检查发送方是否给当前合约授权足够数量的erc20 token
          IERC20 token = IERC20(_token);
          require(token.allowance(msg.sender, address(this)) >= _amount, "need to approve");
          //计算每个地址转账的平均数量
          uint256 avgAmount = _amount / _toAddresses.length;
          for(uint32 i = 0; i < _toAddresses.length; i++){
               token.transferFrom(msg.sender, _toAddresses[i], avgAmount);
          }
          return true;
     }

     // erc20 token 一对多批量转账，指定 token 数量
     function transferDiffERC20ToAll(
          address _token, 
          address payable[] calldata _toAddresses, 
          uint256[] calldata _values
          ) ownable public returns(bool){
          require(_toAddresses.length > 0, "invalid receiver");
          require(_values.length >= _toAddresses.length , "invalid values");
          uint256 sum = getSum(_values);
          IERC20 token = IERC20(_token);
          require(token.allowance( msg.sender , address(this)) >= sum, "need to approve");
          for(uint32 i = 0; i < _toAddresses.length; i++){
               token.transferFrom(msg.sender, _toAddresses[i], _values[i]);
          }
          return true;
     }

     function getSum(uint256[] calldata _amountArr) public pure returns(uint256 sum){
          for(uint32 i = 0; i < _amountArr.length; i++){
               sum = sum + _amountArr[i];
          }
     }

}
```

### V2

```js

```
## Hardhat 脚本

### 部署合约

```ts
import { ethers } from "hardhat";

//部署批量转账合约
async function main() {
  
  const accounts = await ethers.getSigners()
  const balance = await ethers.provider.getBalance(accounts[0].address);
  console.log(`account : ${accounts[0].address} , balance : ${balance}， eth : ${ethers.formatEther(balance.toString())}`)

  const batchTransferFactory = await ethers.getContractFactory("BatchTransferEth");
  const batchTransferContract = await batchTransferFactory.deploy();
  const batchTransferAddress = await batchTransferContract.getAddress();
  console.log(`deploy contract ${batchTransferAddress}`) 
  
}

// We recommend this pattern to be able to use async/await everywhere
// and properly handle errors.
main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```

### 调用合约

```ts
import { ethers } from "hardhat";

//调用批量转账
async function main() {
  
  const accounts = await ethers.getSigners()
  const balance = await ethers.provider.getBalance(accounts[0].address);
  console.log(`account : ${accounts[0].address} , balance : ${balance}， eth : ${ethers.formatEther(balance.toString())}`)

  //上一步中部署成功的合约地址
  const contractAddr = "0x0000";
  //合约batchTransferETHToAll函数的参数
  const _toAddresses = ["0xaddr1", "0xaddr2"];
  
  //根据solidity源文件初始化 factory 实例，并关联已部署的合约实例
  const batchTransferFactory = await ethers.getContractFactory("BatchTransferEth");
  const batchTransferContract = await batchTransferFactory.attach(contractAddr);

  //调用合约batchTransferETHToAll函数，并指定eth数量
  const result = await batchTransferContract.batchTransferETHToAll(_toAddresses, {value: ethers.parseEther("0.01")});

  //获取交易hash
  const wethTxReceipt = await result.wait();
  console.log(`call transfer result : ${result.hash}`)
  
}

// We recommend this pattern to be able to use async/await everywhere
// and properly handle errors.
main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```

## 业务场景

### 空投

# *参考链接*

+ [ethereum batch trasfer](https://github.com/xingzjx/blockchain/blob/master/ethereum/ethereum%20batch%20trasfer.md)