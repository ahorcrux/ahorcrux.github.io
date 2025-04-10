---
title: 
author: since2014
date: 2024-03-02 21:18:00 +0800
categories: [Web3, Project]
tags: [blockchain, web3, crypto]
render_with_liquid: false
---

## 介绍

+ [官网地址](https://fuel.network/)
+ [文档地址](https://docs.fuel.network/)
+ [Github](https://github.com/FuelLabs/fuel-core)
+ [Blog](https://mirror.xyz/0x100dafC2b9604807c4bb28F6826E27626040f8ee)
+ [Discord](https://discord.com/invite/xfpK4Pe)
+ [X](https://twitter.com/fuel_network)
+ [开发论坛](https://forum.fuel.network/)

## 搭建节点

+ [官方教程](https://docs.fuel.network/guides/running-a-node/)

### 节点类型

+ 本地节点
+ 测试网节点

### 环境准备

+ 安装Rust
    ```shell
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
    ```
  
+ 安装Fuel工具链
  
    ```shell
    curl https://install.fuel.network | sh
    ```

### 安装方式

+ Fuel工具链
  
  安装完 Fuel 工具链后，相关命令在 ~/.fuel/bin/ 目录下

+ Sepolia测试网密钥
  
  可在 [Infura](https://app.infura.io/) 或 [Alchemy](https://www.alchemy.com/) 平台申请

+ 生成P2P密钥
  
  进入 ~/.fuel/bin/ 目录执行命令:
  ```shell
  fuel-core-keygen new --key-type peering
  ```

+ 配置
  
  下载官方Github中最新版本的配置文件 [chainConfig.json](https://github.com/FuelLabs/fuel-core/blob/v0.22.1/deployment/scripts/chainspec/beta_chainspec.json) 

+ 运行
  ```shell
  fuel-core run \
    --service-name {ANY_SERVICE_NAME} \
    --keypair {P2P_SECRET} \
    --relayer {ETH_RPC_ENDPOINT}\
    --ip 0.0.0.0 --port 4000 --peering-port 30333 \
    --db-path  ~/.fuel_beta5 \
    --chain ./chainConfig.json \
    --utxo-validation --poa-instant false --enable-p2p \
    --min-gas-price 1 --max-block-size 18874368  --max-transmit-size 18874368 \
    --reserved-nodes /dns4/p2p-beta-5.fuel.network/tcp/30333/p2p/16Uiu2HAmSMqLSibvGCvg8EFLrpnmrXw1GZ2ADX3U2c9ttQSvFtZX,/dns4/p2p-beta-5.fuel.network/tcp/30334/p2p/16Uiu2HAmVUHZ3Yimoh4fBbFqAb3AC4QR1cyo8bUF4qyi8eiUjpVP \
    --sync-header-batch-size 100 \
    --enable-relayer \
    --relayer-v2-listening-contracts 0x557c5cE22F877d975C2cB13D0a961a182d740fD5 \
    --relayer-da-deploy-height 4867877 \
    --relayer-log-page-size 2000

  ```

# *参考链接*

+ 
