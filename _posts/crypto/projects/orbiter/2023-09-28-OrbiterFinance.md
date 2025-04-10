---
title: 跨链桥（Bridge）| OrbiterFinance
author: since2014
date: 2023-09-28 13:53:20 +0800
categories: [Web3, Project]
tags: [blockchain, web3, orbiter, bridge]
render_with_liquid: false
---
# Orbiter Finance 跨链机制

## 概述

> *Orbiter Finance aims to solve the cross-rollup problems instead of the cross-chain issues.*
> 
> *Orbiter Finance 要解决的是Rollup跨链问题，而不是跨（异构 Lyaer1 <> Layer1）链问题。*

Orbiter Finance 是一个专注于以太坊 ***跨 rollup 的去中心化交叉汇总桥*** 解决方案，基于 「***提前信任 + 争议仲裁***」的这样一个解决方案，并且依赖于以太坊网络自身的安全性能。

支持的链包含：*Ethereum, StarkNet, zkSync, Loopring, Arbitrum, Arbitrum Nova, Optimism, Polygon, BNB Chain, ZKSpace, Immutable X, dYdX, Metis and Boba*

## 特性

+ **安全性**
  
  > **水桶理论**：*Vitalik提出了一个概念，叫做共享安全性，即跨链协议的安全性能的上限，是由安全性较低的那条链来决定的*
  > 
  > **51%攻击**
  
  继承了以太坊网络的安全性，没有像其他基于Rollup技术的跨L1桥存在的风险。

+ **便宜、即时**
  
  该协议中的 **Sender** 和 **Maker** 都是 ***EOA*** 类型地址，即资产的转移发生在 EOA 地址之间，而且不是合约地址，也是使得手续费、速度更快且没有合约漏洞的风险。

+ **支持ETH原生资产**
  
  协议资产转移基于流动性的方式而不是 mint / burn等，支持 ETH 原生资产。

## 跨链机制

### 角色

+ **Sender**
  
  发起跨链转账的人，跨链的需求方。

+ **Maker**
  
  流动性提供者，Sender 的对手方，即跨链服务的承接方。在Orbiter协议中，会给Maker提供一个客户端。当然Maker自己也可以去部署一个客户端，目的是实现回款流程的自动化。
  
  [点击查看如何设置Maker节点](https://docs.orbiter.finance/makersystem)

当 Sender 发起转账时，Maker 为其提供流动性，而智能合约则确保整个过程的安全。Maker在创建时，需要设置服务费规则以及存入多余的保证金。

### 合约设计

如何防止Maker作恶呢？

Orbiter 采用了「提前信任 + 争议仲裁」的这样一个解决方案。Orbiter 默认是信任 Maker 的，默认这些 Maker 会正确的处理资产，会把相应的资产返回给用户，但是 Maker 存在作恶的可能，例如收到跨链用户的资产后，把用户的资产扣下，并不在目标链上给用户返回资产。

Orbiter 采用了一套去中心化机制，主要通过三个合约即 MDC、EBC 和 SPV 去实现防止 Maker 作恶

+ **MDC** *('Maker' Deposit Contract)*
  
  保证金合约，其实就是一个 maker，就是 maker 注册的一个合约。然后在这个合约里面，会负责创建它的跨链规则服务，然后还要存放一些 maker 保证金，还有就是后续为 sender 和 maker 提供一个仲裁处理的规则
  
  保管 Maker 的保证金以及处理 Sender 的资金返还和补偿

+ **EBC** *(Event Binding Contract)*
  
  事件绑定合约，用于证明源网络和目标网络上交易的有效性。
  
  主要的运用其实还只是在资产转移方面，所以这个 EBC 合约里面其实会对发起链，就是 sender 给予 maker 的这笔交易，通过来源链交易去推算出目标链交易

+ **SPV** *(Simple Payment Verification)
  
  一个简单的交易验证合约，用于证明源网络上交易是否真的存在。证明在 Orbiter 支持的网络上存在 Tx。Orbiter 需要为每个支持的网络开发 SPV。

### 运作流程

![Orbiter流程图](/img/crypto/p_orbiter_a_process.png)

+ **正确的流程**
  
  1. Maker需要在其EOA地址中预留110-180ETH作为流动性，并在MDC合约中设置代扣费（固定费用）、交易费（0.04% ~ 0.3%）、支持的 Token 类型并存入超额保证金；
  
  2. Sender在 Source Network 发起跨链请求（包含Token类型、数量以及 Target Network 的安全码），并将Token转移到Maker的EOA地址；
  
  3. Maker监听转账事件，确认在 Source Network 上收到对应数量Token；
  
  4. Maker在 Target Network 上将对应跨链数量的Token转到Sender目标地址；
  
  5. Sender确认在 Target Network 上收到对应数量Token；

+ **存在争议的流程**
  
  1. 发送方需要向SPV提供相关的Source Tx；
  
  2. 发送方通过Orbiter的MDC合约发起仲裁；
  
  3. MDC从SPV获得Source Tx的存在证明，并知道Source Tx已经发生在源网络上；
  
  4. MDC从EBC获取Source Tx的有效性证明。MDC 将知道 Source Tx 是合法地基于 Orbiter 的规则；源 Tx 是从发送者发送到轨道飞行器的制造者之一并带有合法的安全代码；
  
  5. MDC 会将本次仲裁设置为待处理案件，并等待 Maker 提供 Target Tx，时间为 0.5-3 小时（此功能可以在 Maker 客户端聚合，因此 Maker 不会错过）。如果Maker能够提供正确的Target Tx，MDC将从EBC获得有效性证明并知道Target Tx与Source Tx匹配。MDC 将关闭此仲裁并向发送者显示目标 Tx。但是，如果Maker在0.5-3小时后无法提供相关Target Tx，则Sender可以申请提现，然后进入下一步；
  
  6. MDC开始补偿Sender；
  
  7. MDC 将把代币和补偿（约 15 美元）发送回 MDC 部署的域上的发件人。返还的代币和补偿来自 Maker 的保证金；

## 技术实现

### 项目模块
  
  1. [**OrbiterModule**](https://github.com/Orbiter-Finance/OrbiterModule)
     
     由js开发的docker项目，Orbiter 模块由 Maker 服务和 Dashboard 服务组成。Maker Service 依赖于 chaincore 库。当 chaincore 服务检测到 Maker 的链上地址发生交易时，它将通知 chaincore 订阅者。Dashboard 服务主要提供 Maker 管理页面，用于检查交易历史记录、实时余额和匹配状态等
  
  2. [**Dashboard**](https://github.com/Orbiter-Finance/OrbiterModule)
     
     通过RPC数据建立数据库，以统计表格展示Maker地址的运行状况。Navigator通过将此工具与Metamask结合，可以及时调整策略。MakerTransactionData 可以检索链上数据并处理交易数据。主要用于将链上所有与Maker相关的交易与本地数据库同步，并将交易推送到其他服务。
  
  3. **ChainCore**
     
     chaincore是npm上开源的javascript公共库，用于封装对各种公链区块/交易等数据的某些访问、交易查询、余额查询、日志等操作
  
  4. [**Orbiter X Contract**](https://github.com/Orbiter-Finance/xvm)
     
     Orbiter X合约是一款支持跨地址、跨币种、批量返还的合约。可以向第三方调用，将大大提高访问效率
  
  5. [**OB_ReturnCabin**](https://github.com/Orbiter-Finance/OB_ReturnCabin)
     
     Solidity开发的一些列合约，包括：MDC 、EBC 、SPV


## 参考来源

+ [Orbiter Finance Docs](https://docs.orbiter.finance/technology)

+ [跨链公开课第8讲《代表跨链桥项目分析》 - YouTube](https://www.youtube.com/watch?v=NVqkRswuOy0&list=UULFw_CmiB5oEHVQUKJ2Ac-erg&index=12)

+ [深入解读Orbiter：跨链桥变身 将成为通用以太坊基础协议_手机新浪网](https://finance.sina.cn/blockchain/2023-08-02/detail-imzeuppa4192381.d.html)
