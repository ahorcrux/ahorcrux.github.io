---
title: 
author: since2014
date: 2023-11-1 20:03:00 +0800
categories: [Technology, Blockchain]
tags: [blockchain, web3, hardhat, ether.js]
render_with_liquid: false
---

Contract实例化时，是选择provider还是signer?
https://learnblockchain.cn/docs/ethers.js/api-contract.html#providers-signers

https://docs.ethers.org/v5/single-page/

```js
// 只读; 通过 Provider连接，允许以下操作:
// - 访问常量函数 constant function
// - 查询过滤器
// - 对于 non-constant methods 填充未签名的交易
// - 对于non-constant估算Gas(作为匿名发送者)
// - 对于non-constant methods静态调用 (作为匿名发送者)
const erc20 = new ethers.Contract(address, abi, provider);

// 可读写; 通过Signer连接，允许以下操作:
// - 任何只读的操作 (作为Signer而非匿名者)
// - 为non-constant functions发送交易
const erc20_rw = new ethers.Contract(address, abi, signer);

//先provider，providerOrSigner是不可变的，如果需要更改其他account or provider，可以通过connect实现
 const tokenContract = new ethers.Contract(contractAddress, tokenAbi, connection);
var signer = new ethers.Wallet(privateKey, connection);
const txSigner= tokenContract.connect(signer);
const transaction = await txSigner.transfer(to,address,amount)
```

## 参考来源

+ [登链社区 - Ether.js v5 中文文档](https://learnblockchain.cn/ethers_v5/)
+ [登链社区 - Eether.js中文文档](https://learnblockchain.cn/docs/ethers.js/getting-started.html)