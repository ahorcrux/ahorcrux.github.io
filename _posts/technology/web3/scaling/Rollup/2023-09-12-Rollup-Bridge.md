---
title: 跨链桥（Bridge）| Rollup 桥
author: since2014
date: 2023-09-12 06:30:00 +0800
categories: [Technology, Blockchain]
tags: [blockchain, web3, rollup, bridge]
render_with_liquid: false
---

## Rollup Bridge

Rollup 的安全性是锚定在它所在的 L1 上的（例如 Optimism 锚定在 Ethereum 上）：你要针对 L2 的交易进行审查攻击，等同于對 L1 交易进行审查攻击；你要 re-org L2 ，进行双花攻击，等同于你要 re-org L1。

Rollup Bridge 是 L1 和 L2 互动的管道，你要从 L1 送消息到 L2 合约或是你要 deposit ETH 到 L2，都是通过 Rollup Bridge 来帮忙把消息 relay 过去；反之亦然，你要从 L2 送消息到 L1 合约或是你要 withdraw ETH 回 L1，也是通过 Rollup Bridge。Rollup Bridge 的安全性和 L2 交易的安全性是一样的，不会因为它多跨到了 L1 所以更不安全。

所以 Rollup Bridge 代表的是什麼意思？Rollup Bridge 提供了一個安全、去中心化的方式让 L1 和 L2 能够进行互动、让 L1、L2 的资产能够互相转移。
註：Rollup Bridge 不是自然而然就会出現的東西，它不会因为你今天做了一個 Rollup，就自动生出 Rollup Bridge 的功能，而是要搭配你的 Rollup 协议来打造你的 Rollup Bridge。

如果你通过跨链桥用 MPC 的方式来执行跨链交易，你会需要相信参与 MPC 的节点。但在 Rollup Bridge 里沒有这样的角色，通过 Rollup Bridge 送消息就跟送 Rollup 交易一样，其安全性和抗审查性是受 Rollup 本身所保障，所以才会说 Rollup Bridge 是去中心化的方式。


## 参考链接

+ [Rollup Bridge 介绍]https://medium.com/imtoken/rollup-bridge-%E4%BB%8B%E7%B4%B9-%E4%B8%80-maker-dai-bridge-678c62228eb5