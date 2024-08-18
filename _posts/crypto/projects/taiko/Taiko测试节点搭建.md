---
title: 
author: since2014
date: 2023-10-28 18:02:00 +0800
categories: [Web3, Project]
tags: [blockchain, web3, crypto]
render_with_liquid: false
---

## 介绍

+ [官网地址](https://taiko.xyz/)
+ [文档地址](https://docs.taiko.xyz/start-here/getting-started/)
+ [官网地址](https://www.berachain.com/)


## 运行节点

### 环境准备

### L1节点(Holesky)

+ 本地运行

[官方教程](https://docs.taiko.xyz/guides/run-a-holesky-node/)

需要至少300G空间

1. 拉代码
     ```shell
     git clone https://github.com/eth-educators/eth-docker
     cd eth-docker
     ```

2. 配置 eth-docker
     ```shell
     ./ethd config
     ```
     选择 Holesky Testnet -> 选择 Ethernum Rpc Node -> 选择 Lighthouse -> 选择 Geth ；
     MEV BOOT：No 
     Grafana dashboards：Yes 

3. 修改配置文件
     ```shell
     cp default.env .env
     ```
     ```shell
     COMPOSE_FILE=lighthouse-cl-only.yml:geth.yml:el-shared.yml
     ```
     ```shell
     ARCHIVE_NODE=true
     ```
4. 运行
     ```shell
     ./ethd up
     ```

+ 第三方

1. Alchemy
2. Infura
3. Quick Node

### 运行Taiko节点

  [官方教程](https://docs.taiko.xyz/guides/run-a-taiko-node/)

+ 安装 stn 

     [Github地址](https://github.com/d1onys1us/stn)

1. Cargo 安装

```shell
cargo install stn
```

+ 通过stn安装taiko

```shell
stn install
```
+ 配置

配置L1节点的http url 和 wss url

```shell
stn config full
```

+ 启动

```shell
stn up
```
+ 验证

```shell
stn status
```

```shell
stn dashboard
```

### 提案节点

[设置为提案节点](https://docs.taiko.xyz/guides/enable-a-proposer/)

+ stn设置

```shell
stn config proposer
```

### 设置验证节点

[设置为验证节点](https://docs.taiko.xyz/guides/enable-a-prover/)




# *参考链接*

+ https://medium.com/@jiamigou/%E5%8A%A0%E5%AF%86%E7%8B%97%E6%95%B4%E7%BC%96%E7%A9%BA%E6%8A%95%E7%AC%AC224%E7%AF%87-%E5%A6%82%E4%BD%95%E5%9C%A8taiko-alpha-3%E4%B8%8A%E9%83%A8%E7%BD%B2%E8%8A%82%E7%82%B9-aef4f14c32d6

+ [Sepolia L1节点](https://www.youtube.com/watch?v=7Lg_cY7iP2o)