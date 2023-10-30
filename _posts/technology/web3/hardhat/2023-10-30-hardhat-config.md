---
title: Hardhat 教程 | hardhat.config 配置项
author: since2014
date: 2023-10-30 16:41:00 +0800
categories: [Technology, Blockchain]
tags: [blockchain, web3, hardhat, 使用技巧]
render_with_liquid: false
---

### Solidity

配置Solidity编译器版本

+ 配置单个编译器版本
  
  ```js
  solidity: {
    version: "0.8.19",
    settings: {
      optimizer: {
        enabled: true,
        runs: 1000,
      },
    },
  },
  ```
+ 配置多个编译器版本
  
  ```js
  solidity:{
        compilers:[
            {version: "0.8.0"},
            {version: "0.4.2"},
        ]
    },
  ```

### Network

配置网络

```js
networks: {
    hardhat: {
    },
    rinkeby: {
      url: "https://eth-mainnet.alchemyapi.io/v2/123abc123abc123abc123abc123abcde",
      accounts: [privateKey1, privateKey2, ...]
    },
    rinkeby2: {
      url: "https://eth-mainnet.alchemyapi.io/v2/123abc123abc123abc123abc123abcde",
      accounts: [privateKey1, privateKey2, ...]
    }
  },
```
