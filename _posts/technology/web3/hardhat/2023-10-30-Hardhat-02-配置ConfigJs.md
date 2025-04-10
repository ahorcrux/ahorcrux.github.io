---
title: Hardhat 学习笔记 | [02] 配置文件 Hardhat.config.js
author: since2014
date: 2023-10-30 16:41:00 +0800
categories: [Technology, Blockchain]
tags: [blockchain, web3, hardhat, 使用技巧]
render_with_liquid: false
---

## Hardhat.Config

+ js版本
  
  ```js
  require("@nomicfoundation/hardhat-toolbox");
  require("dotenv").config();
  const { PRIVATE_KEY } = process.env;

  module.exports = {
    solidity: {
      version: '0.8.17',
      version: '0.8.18',
    },
    networks: {
      linea_testnet: {
        url: `https://rpc.goerli.linea.build/`,
        accounts: [PRIVATE_KEY],
      },
      linea_mainnet: {
        url: `https://rpc.linea.build/`,
        accounts: [PRIVATE_KEY],
      },
    },
  };
  ```
+ ts版本

  ```ts
  import "@nomicfoundation/hardhat-toolbox";
  import * as dotenv from 'dotenv';
  import { HardhatUserConfig } from "hardhat/config";

  //如果是import "dotenv/config" ，则不需要该行代码
  dotenv.config();

  //create simple task
  // task("accounts", "Prints the list of accounts", async (taskArgs, hre) => {
  //   const accounts = await hre.ethers.getSigners();

  //   for (const account of accounts) {
  //     console.log(account.address);
  //   }
  // });

  const config: HardhatUserConfig = {
    solidity: {
      version: "0.8.20",
      settings: {
        optimizer: {
          enabled: true,
          runs: 200,
        },
        evmVersion: "london"
      }
    },
    networks: {
      hardhat: {
      },
      scroll_sepolia: {
        url: process.env.RPC_URL_TEST_SCROLL || "",
        accounts: process.env.PRIVATE_KEY !== undefined ? [process.env.PRIVATE_KEY] : [],
      },
      sepolia: {
        url: process.env.RPC_URL_SEPOLIA || "",
        accounts: process.env.PRIVATE_KEY !== undefined ? [process.env.PRIVATE_KEY] : [],
      },
    },
  };

  export default config;
  ```
## Solidity

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

## Network

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
