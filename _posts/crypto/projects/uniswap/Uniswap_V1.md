---
title: 
author: since2014
date: 2023-10-28 18:02:00 +0800
categories: [Web3, Project]
tags: [blockchain, web3, dex，uniswap]
render_with_liquid: false
---

## 前言

2018年11月推出 V1

## 恒定乘积自动做市商

Constant-product Automated Market Maker

公式： x * y = k

## 无偿损失

Impermanent Loss

假设有 ETH/USDC 这样一个池子，初始流动性 x * y = k 为 100(ETH) * 30000(USDC) = 3000000 。
用户A添加流动性，10(ETH) * 3000(USDC) , 流动性占比 1% ， 此时 ETH价格为 300USDC/ETH ，用户A持仓换算成USDC价值为 10 * 300 + 3000 = 6000USDC，
一段时间后，市场ETH价格上涨至1200，流动池 x1 * y1 = k 为 50(ETH) * 60000(USDC) = 3000000 。
此时用户A的流动性持仓就变成 5(USDC) + 6000(USDC)，此时用户A流动性LP持仓换算成USDC价值为 5 * 1200 + 6000 = 12000 USDC。
如果用户A不提供流动性，只是持有原先10ETH和3000USDC，换算成USDC价值为 10 * 1200 + 3000 = 15000 USDC。
这里的 15000 - 12000 = 3000 USDC 价值折损即为无偿损失。

## 滑点
