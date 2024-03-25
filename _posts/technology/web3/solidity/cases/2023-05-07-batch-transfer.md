---
title: Solidity 用例 | 批量转账
author: since2014
date: 2023-06-17 11:26:23 +0800
categories: [Technology, Blockchain]
tags: [blockchain, web3, solidity]
render_with_liquid: false
---

## 合约代码

```js
//SPDX-License-Identifier: MID
pragma solidity ^0.8.14;

contract BatchTransferEth {
     
     address payable public owner;

     modifier ownable(){
          require(msg.sender == owner, "not owner");
          _;
     }

     constructor() payable {
          owner = payable(msg.sender);
     }

     //批量转ETH给所有地址，数量平均分配，调用时指定的 value 值即总的 ETH 数量
     function batchTransferETHToAll(address[] calldata _toAddresses ) payable ownable public returns (bool){
          require(_toAddresses.length > 0, "toAddresses is empty");
          uint avgEth = address(this).balance / _toAddresses.length; 
          for(uint32 i = 0; i < _toAddresses.length; i++){
               payable(_toAddresses[i]).transfer(avgEth);
          }
          return true;
     }
}
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

## *参考链接*

+ [ethereum batch trasfer](https://github.com/xingzjx/blockchain/blob/master/ethereum/ethereum%20batch%20trasfer.md)