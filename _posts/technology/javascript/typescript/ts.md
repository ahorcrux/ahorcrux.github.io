---
title: 
author: since2014
date: 2023-10-28 18:02:00 +0800
categories: [Technology, TypeScript]
tags: [typescript]
render_with_liquid: false
---

## 安装

```shell
npm install -g typescript

```


Hardhat中使用TypeScript

1. 安装
   
   ```shell
   npm install --save-dev ts-node typescript tslint
   ```
   

   测试相关包

   ```shell
   npm install --save-dev chai @types/node @types/mocha @types/chai
   ```

2. 通过npx hardhat 创建项目时，选择 Create a TypeScript Project


## Type script + Hardhat + Yarn

参考[官方文档](https://hardhat.org/hardhat-runner/docs/guides/typescript)

1. 安装依赖
   
   ```shell
   yarn add --dev ts-node typescript
   ````

   ```shell
   yarn add --dev hardhat
   ````   
2. 通过npx hardhat 创建项目时，选择 Create a TypeScript Project
   
   ```shell
   npx hardhat init
   ```

3. 其他依赖
   
   ```shell
   yarn add --dev @nomicfoundation/hardhat-toolbox @nomicfoundation/hardhat-ignition @nomicfoundation/hardhat-ignition-ethers @nomicfoundation/hardhat-network-helpers @nomicfoundation/hardhat-chai-matchers @nomicfoundation/hardhat-ethers @nomicfoundation/hardhat-verify chai@4 ethers hardhat-gas-reporter solidity-coverage @typechain/hardhat typechain @typechain/ethers-v6
   ```

# 参考来源
+ [官方文档](https://www.tslang.cn/docs/release-notes/typescript-3.0.html)
+ [TypeScript 入门教程](https://ts.xcatliu.com/)