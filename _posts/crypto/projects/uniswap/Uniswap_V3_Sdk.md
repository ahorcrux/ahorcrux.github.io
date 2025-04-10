---
title: 
author: since2014
date: 2023-10-28 18:02:00 +0800
categories: [Web3, Project]
tags: [blockchain, web3, crypto]
render_with_liquid: false
---

## 注意事项

Multicall.sol


v3-sdk
Uniswap V3 SDK并不提供直接创建流动性池的功能。该SDK主要用于帮助开发者进行价格计算，范围订单，路径生成等操作

创建流动性池是直接通过交互与Uniswap V3 Factory合约实现的

```ts

const { Pool, Token, CurrencyAmount, Trade, Percent, Route, TokenAmount } = require('@uniswap/v3-sdk')
const { ethers } = require('ethers')

// 首先，你需要定义你的token
const tokenA = new Token(1, '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2', 18, 'WETH', 'Wrapped Ether')
const tokenB = new Token(1, '0x6B175474E89094C44Da98b954EedeAC495271d0F', 18, 'DAI', 'Dai Stablecoin')

// 然后，你需要创建池子实例
const pool = new Pool(tokenA, tokenB, 3000, '997207053370399', '1923620462213096887541', '430070')

// 之后，你可以使用这个池子来进行交易
const amountIn = CurrencyAmount.fromRawAmount(tokenA, '1000000000000000000') // 1 WETH
const trade = Trade.exactIn(new Route([pool], tokenA, tokenB), amountIn)

console.log(trade.executionPrice.toSignificant(6)) // 执行价格
console.log(trade.nextMidPrice.toSignificant(6)) // 下一个中间价格


```

## V3 合约部署地址

https://docs.uniswap.org/contracts/v3/reference/deployments

## V3-SDK使用示例

1. 安装v3-sdk
   
   ```shell
   npm install @uniswap/v3-sdk @uniswap/sdk-core
   ```

2. 创建流动性池
   
   ```ts
   const { Token, Pool } = require('@uniswap/v3-sdk')
   const { ChainId, TokenAmount } = require('@uniswap/sdk-core')

   // define tokens
   const tokenA = new Token(ChainId.MAINNET, '0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48', 6) // USDC
   const tokenB = new Token(ChainId.MAINNET, '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2', 18) // WETH

   // define pool
   const pool = new Pool(tokenA, tokenB, 3000, '791210831791666', '1360787597443') 

   ```
3. 添加流动性
   
   ```ts
   const { Position } = require('@uniswap/v3-sdk')

   // define position
   const position = Position.fromAmounts({
     pool,
     tickLower: -60, // must be a multiple of the pool's tick spacing
     tickUpper: 60, // must be a multiple of the pool's tick spacing
     amount0: new TokenAmount(tokenA, '1000000'), // desired amount of token0
     amount1: new TokenAmount(tokenB, '1000000'), // desired amount of token1
   })
   ```

4. Token兑换
   
   ```ts
   const { Route, Trade, TradeType, Percent } = require('@uniswap/v3-sdk')
   const { Fraction } = require('@uniswap/sdk-core')

   // define route
   const route = new Route([pool], tokenA, tokenB)

   // define trade
   const trade = new Trade(route, new TokenAmount(tokenA, '1000000'), TradeType.EXACT_INPUT)

   // slippage tolerance
   const slippageTolerance = new Percent('50', '10000') // 0.50%

   // amount out, accounting for slippage
   const amountOutMin = trade.minimumAmountOut(slippageTolerance).raw

   ```
5. 移除流动性
   
   ```ts
   const { TickMath } = require('@uniswap/v3-sdk')

   // calculate amounts to remove
   const sqrtRatioX96 = TickMath.getSqrtRatioAtTick(position.tickLower)
   const amount0ToRemove = position.amount0.minus(position.amount0Remaining(sqrtRatioX96))
   const amount1ToRemove = position.amount1.minus(position.amount1Remaining(sqrtRatioX96))

   ```

## 代码

https://cloud.tencent.com/developer/article/1833176

+ Hardhat引入v3-sdk依赖
  
  ```shell
  npm i --save @uniswap/v3-sdk
  npm i --save @uniswap/sdk-core
  ```

+ Hardhat引入v3合约
  
  ```
  npm install @uniswap/v3-periphery @uniswap/v3-core
  ```

## 参考来源

+ https://y1cunhui.github.io/uniswapV3-book-zh-cn/docs

