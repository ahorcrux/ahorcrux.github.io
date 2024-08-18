---
title: 
author: since2014
date: 2024-03-02 21:18:00 +0800
categories: [Web3, Project]
tags: [blockchain, web3, crypto]
render_with_liquid: false
---

## 介绍

+ [官网地址](https://selfchain.xyz/)
+ [文档地址](https://docs.selfchain.xyz/)
+ [Github](https://github.com/FuelLabs/fuel-core)
+ [Discord](https://discord.gg/selfchainxyz)
+ [X](https://twitter.com/selfchainxyz)

## 搭建节点

+ [官方教程](https://docs.selfchain.xyz/nodes-and-validators/node-setup-guide)

### 环境准备

### 安装运行

+ 下载
  
```shell
wget -q -O selfchaind-linux-amd64 https://1501792788-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FcIZFCZY4EPKDYaPcDZLG%2Fuploads%2FMuuuXZOR6UJKBlYkv27C%2Fselfchaind-linux-amd64?alt=media
```

添加到bin目录

sudo cp selfchaind /usr/local/bin/

sudo chmod +777 selfchaind

+ 配置

配置Config.toml

https://github.com/hotcrosscom/selfchain-genesis/blob/main/networks/devnet/peers.toml

sudo vi ~/.selfchain/config/config.toml

seeds = "94a7baabb2bcc00c7b47cbaa58adf4f433df9599@157.230.119.165:26656,d3b5b6ca39c8c62152abbeac4669816166d96831@165.22.24.236:26656,35f478c534e2d58dc2c4acdf3eb22eeb6f23357f@165.232.125.66:26656"

persistent_peers = "94a7baabb2bcc00c7b47cbaa58adf4f433df9599@157.230.119.165:26656,d3b5b6ca39c8c62152abbeac4669816166d96831@165.22.24.236:26656,35f478c534e2d58dc2c4acdf3eb22eeb6f23357f@165.232.125.66:26656"

[p2p]
laddr = "tcp://0.0.0.0:26666"

pprof_laddr = "localhost:6062"

[rpc]
laddr = "tcp://0.0.0.0:26667"


配置app.toml


sed -i 's/minimum-gas-prices = "0stake"/minimum-gas-prices = "1uself"/' /home/ubuntu/.selfchain/config/app.toml
sed -i 's/denom-to-suggest = "uatom"/denom-to-suggest = "uself"/' /home/ubuntu/.selfchain/config/app.toml
sed -i 's/chain-id = "selfchain"/chain-id = "self-dev-1"/' /home/ubuntu/.selfchain/config/client.toml

pruning = "custom"

pruning-keep-recent = "10"
pruning-keep-every = "1000"
pruning-interval = "10"

根据服务器情况修改端口

[api]


enable = true

swagger = true

address = "tcp://0.0.0.0:1314"
[grpc]

enable = true

address = "0.0.0.0:9095"

[grpc-web]

enable = true

address = "0.0.0.0:9190" 
  
创建钱包

keplr

cli创建
selfchaind keys --home $SELF_CHAIN_HOME --keyring-backend file --keyring-dir keys add <key_name>


用助记词恢复

selfchaind keys add account-test-18 \
--recover \
--keyring-backend file --keyring-dir /home/ubuntu/.selfchain/keys

领取测试币

https://superboard.xyz/quests/intro-to-self-chain's-incentivised-testnet-ii/tasks

+ 运行

nohup selfchaind start --x-crisis-skip-assert-invariants >> selfchain.out 2>&1 &

# *参考链接*

+ 
