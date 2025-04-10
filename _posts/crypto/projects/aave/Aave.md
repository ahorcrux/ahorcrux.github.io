---
title: 
author: since2014
date: 2023-10-28 18:02:00 +0800
categories: [Web3, Project]
tags: [blockchain, web3, crypto]
render_with_liquid: false
---

## 基本概念

### 健康因子
健康因子 = 抵押资产价值*清算阈值/借贷金额 = 清算阈值 / 当前借贷金额使用率 = 当前LTV / Liquidation Threshold

### LTV
贷款价值比：用来衡量抵押物和贷款的比值，AAVE中LTV数值由抵押物确定

比如用抵押了100元资产，LTV为80%，则用户可以贷出80元资产

### Liquidation Threshold
清算阈值
当借款用户抵押的资产价值总额低于这个阈值的时候就会被清算，比如用户抵押100个ETH贷出800个USDT，清算阈值为80%，如果价格波动，100个ETH的总价值低于800*80% = 640 个USDT时，抵押资产就会倍清算。

### 清算价格
清算价格即抵押物能借贷的最大金额 / 抵押物数量
清算价格 = 清算阈值 * 抵押物总金额 / 抵押物数量
