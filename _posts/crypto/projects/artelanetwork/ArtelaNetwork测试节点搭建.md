---
title: 
author: since2014
date: 2024-02-22 14:56:00 +0800
categories: [Web3, Project]
tags: [blockchain, web3, crypto]
render_with_liquid: false
---

## 介绍

+ [官网](https://artela.network/)
+ [Discord](https://discord.com/invite/artela)
+ [Twitter](https://twitter.com/artela_network)

## 运行节点

[官方教程](https://docs.artela.network/develop/node/run-full-node)

### 环境准备

+ 安装 Golang

     ```shell
     sudo apt-get update
     sudo apt-get install -y make gcc
     wget https://go.dev/dl/go1.20.3.linux-amd64.tar.gz
     sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.20.3.linux-amd64.tar.gz
     export PATH=$PATH:/usr/local/go/bin
     ```
     设置 Go Src 代码目录
     ```shell
     mkdir -p ~/go/src
     ```
     ```shell
     export GOPATH=~/go
     ```

     设置 Go Bin 目录
     ```shell
     mkdir -p ~/go/bin
     ```
     ```shell
     export GOPATH=~/go/bin
     ```

### 安装

1. 源码

     ```shell
     cd $GOPATH/src
     git clone https://github.com/artela-network/artela
     # git clone https://github.com/artela-network/artela-cosmos-sdk
     # git clone https://github.com/artela-network/artela-cometbft
     cd artela

     git checkout main
     make clean && make
     cp ./build/artelad $HOME/go/bin/.
     export PATH=$PATH:$HOME/go/bin
     ```

2. 安装包

     进入 [Github Releases](https://github.com/artela-network/artela/releases) 下载，并复制 artelad 到 /usr/local/bin 目录。

### 配置

[测试网络信息](https://docs.artela.network/develop/node/access-testnet#public-information-on-testnet)

```json
Network Name : Artela TestNet
New RPC URL : https://betanet-rpc1.artela.network
ChainID (optional): 11822
Symbol (optional) : ART
Block Explorer URL (optional): https://betanet-scan.artela.network/
```

+ 设置名称
     VishantiLabs

     ```shell
     artelad init VishantiLabs
     ```
+ 设置 Genesis.json

     下载并复制 [Genesis.json](https://docs.artela.network/assets/files/genesis-314f4b0294712c1bc6c3f4213fa76465.json) 文件

+ 设置持久化节点

     ```shell
     cd $HOME/.artelad/config
     # e.g sed -i 's/seeds = ""/seeds = "ef1777650f2a5f96cfbf2b1b21feb45ef09bbaa4@172.16.10.2:26656,96a8e722f93acacd21baec6db51acd6cc16bbee2@172.16.10.4:26656"/' config.toml
     sed -i 's/seeds = ""/seeds = "9e2fbfc4b32a1b013e53f3fc9b45638f4cddee36@47.254.66.177:26656,aa416d3628dcce6e87d4b92d1867c8eca36a70a7@47.254.93.86:26656"/' config.toml
     ```
+ 设置状态同步

     配置区块 Height 和 Hash (可以从[浏览器获取最新](https://betanet-scan.artela.network/))，以及 Rpc 节点

     ```shell
     cd $HOME/.artelad/config
     sed -i 's/enable = false/enable = true/' config.toml
     sed -i 's/trust_height = 0/trust_height = 4010625/' config.toml
     sed -i 's/trust_hash = ""/trust_hash = "abbb42cd460626b2e60f813f569334d6d81fb60d4b07a2a34e43013d5637ae1b"/' config.toml

     # e.g sed -i 's/rpc_servers = ""/rpc_servers = "172.16.10.2:26657,172.16.10.4:26657"/' config.toml
     sed -i 's/rpc_servers = ""/rpc_servers = "47.254.66.177:26657,47.254.93.86:26657"/' config.toml
     ```
+ 设置 app.toml

     根据服务器端口，修改 app.toml 和 config.toml 端口，防止冲突
     根据官方文档给出的建议配置，修改 app.toml 和 config.toml 文件
### 运行

1. 命令行
     ```shell
     export PATH=$PATH:$HOME/go/bin

     nohup artelad start --log_level info > artelad-info.log 2>&1 &
     ```

2. PM2

     root 下执行命令
     ```shell
     sudo apt update
     sudo apt install npm -y
     sudo npm install -g n
     n latest
     hash -r

     npm install pm2@latest -g
     ```

### 设置 Validator

1. 创建账户

     + artelad-account-01
          zjc: test-19
          evm-addr: 0x2Eeb886415168494061286896F6009812F2e4D01
          art-addr: art19m4cseq4z6zfgpsjs6yk7cqfsyhjungpjtkjf8
2. 获取测试币
     在 Discord 中 testnet-faucet 频道，输入 $request 0x2Eeb886415168494061286896F6009812F2e4D01
3. 创建Validator

     ```shell
     artelad tx staking create-validator --amount="100000000000000000uart" --pubkey=$(artelad tendermint show-validator) --moniker=VishantiLabs --commission-rate="0.10" --commission-max-rate="0.20" --commission-max-change-rate="0.01" --min-self-delegation="1" --gas=300000 --chain-id=artela_11822-1 --from=artelad-account-01 --node tcp://47.254.66.177:26657 -y
     
     ```

     ```shell
     artelad tx staking delegate $(artelad keys show wallet --bech val -a) 1000000uart --from wallet --chain-id artela_11822-1 --gas 300000 --node tcp://47.254.66.177:26657 -y
     ```
4. 校验Validator状态

     ```shell
     artelad query tendermint-validator-set | grep "$(artelad tendermint show-address)"
     ```
https://43.198.114.225:8084/

# *参考链接*

+ https://medium.com/@smeb_y/artela-%E8%8A%82%E7%82%B9%E9%83%A8%E7%BD%B2-%E9%AA%8C%E8%AF%81%E8%80%85%E8%8A%82%E7%82%B9%E5%88%9B%E5%BB%BA%E5%9B%BE%E6%96%87%E5%85%A8%E6%95%99%E7%A8%8B-2aebf9af2913