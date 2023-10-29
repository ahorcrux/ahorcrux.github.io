---
title: 公链 | Polygon Edge（Supernet）部署教程
author: since2014
date: 2023-10-25 16:34:00 +0800
categories: [Web3, Project]
tags: [blockchain, web3, polygon, supernet]
render_with_liquid: false
---

## 测试环境配置（Mumbai）

| Key                        | Value                                      |
| -------------------------- | ------------------------------------------ |
| Mumbai RPC                 | https://rpc-mumbai.maticvigil.com          |
| Mumbai Scan                | https://mumbai.polygonscan.com/            |
| SuperNet Chain Id          | 2234                                       |
| SuperNet Rpc Port          | 10001 、10002 、10003 、10004                 |
| 合约部署者                      | 0x454b08980d36511c47d463B3AaD0E9DA1672A665 |
| Proxy-contracts-admin      | 0x654c50049e2f4bfca4cc12c384e1b5003bbcfcb7 |
| Stake Erc20Token           | 0x537a2E0048ae4d2cabfFfD2Fe125Ad5703Ea6553 |
| StakeManager               | 0x41ffd39f1D4215A16C2C5A30884387E033bB5cBc |
| CustomSupernetManager      | 0xD8b4D4A8cAc18630BeEb9a0610D9a8DB6D17EA5D |
| CustomSupernetManagerProxy | 0xE3EdAFD12a5F45A25181A05ec4119Cb6C49fE2Df |

## 部署Supernet

### 创建并配置 Chlid Chain
  
  1. *创建 Node 密钥*
     
     ```shell
     ./polygon-edge polybft-secrets --insecure --data-dir test-chain- --num 4
     ```
     
     输出示例：
     
     ```shell
     [SECRETS GENERATED]
     network-key, validator-key, validator-bls-key
     
     [SECRETS INIT]
     Public key (address) = 0xf0f63Bf5D51fbbD067CA2595EaC9e61333b153FB
     BLS Public key       = 06b0394473ba3adbbc240d0695420a3891b20cfffbb05451959e51632d896e0702d7c86609b47bd08bd12b3d61dc01224c58d30a2bfa4eaee232d639feefdda12b22d23b10fa182f631de7821dfe63a18d521495974e6bf861228bf79918c6bb1252808daf3fce9796ba96742121e6378306eff671efd953f6e60b798657a92c
     Node ID              = 16Uiu2HAmFSJZ6vMCGGmQ5Zi9Z7PJgZLJT1dD9VFxVLLpsbe76sdc
     
     [WARNING: INSECURE LOCAL SECRETS - SHOULD NOT BE RUN IN PRODUCTION]
     
     [SECRETS GENERATED]
     network-key, validator-key, validator-bls-key
     
     [SECRETS INIT]
     Public key (address) = 0x03679198091F9Da027F98598c90e774A2468B901
     BLS Public key       = 028af72071e96e2736ca69c0ce7bd33d86e1ef5450797aa2e95a1206dc35b8fe299d563f3cb458103fea30982cc0590ddaa1300c36df09cc32672554af9a670d157e918fb22f8e7232f3e6e6284e7de2cdf8c27f21af103c35fc22e4d8bc8ca90228e32fc39adaea395d37571de2837687db58026cec38de866d0cfd74b5f500
     Node ID              = 16Uiu2HAmEMfC49EUsEPgZFPcY1dW47odWrxWJ3nArNryvK6UBuFq
     
     [WARNING: INSECURE LOCAL SECRETS - SHOULD NOT BE RUN IN PRODUCTION]
     
     [SECRETS GENERATED]
     network-key, validator-key, validator-bls-key
     
     [SECRETS INIT]
     Public key (address) = 0x5f70789636ec39e589326566E66bf75A177C8a5F
     BLS Public key       = 1916960ca41387e77434a301cf548ab0b73ad99ce87eca74bc389a206a1dcf9a079741c11dbd70fed993f78f93f6452d170c31b7de8c92a37a3564c208c92a39200311eba43b767f297ae577e9350adbcd27e04caa4c586c04b07b3bba43fbf52e0ce5b0d22f97b0ab70691b78761335fe84ab15d5fc18464125aea08e2a7b3b
     Node ID              = 16Uiu2HAmSRUJcmfVNVMQ2nxStP5JKBktKozJdwfWmpJgHJ4kQZJh
     
     [WARNING: INSECURE LOCAL SECRETS - SHOULD NOT BE RUN IN PRODUCTION]
     
     [SECRETS GENERATED]
     network-key, validator-key, validator-bls-key
     
     [SECRETS INIT]
     Public key (address) = 0xee38cB7897D0d66Feb40df0FFB93541EC9794Ae2
     BLS Public key       = 2d376bde47ff7f8f00e89a689e1fd2cd5489f6513aca214fade3fd36c54f88562e26a3e161bbf9df086e51e6d4dbb52a791ed80b8550ae13dd5a21a6c8dec0532a8acdfa7f88ca3d762c1394d87ee0221fac90fa6053c8135477019f7bb2e50f2d8a269d15e619542632be3ba8844da6b4ca5295ac6c066e71b796647aa7a79a
     Node ID              = 16Uiu2HAkuv7PL1ug2Gt9rtbUTtXUw3Tn6vTDX6qNVDEWivWAd1CB
     ```
  
  2. *配置并生成 Genesis.json 文件*
     
     ```shell
     ./polygon-edge genesis --block-gas-limit 5242880 --epoch-size 10 \
         --proxy-contracts-admin 0x454b08980d36511c47d463b3aad0e9da1672a665 \
          --validators "/ip4/127.0.0.1/tcp/30301/p2p/16Uiu2HAmFSJZ6vMCGGmQ5Zi9Z7PJgZLJT1dD9VFxVLLpsbe76sdc:0xf0f63Bf5D51fbbD067CA2595EaC9e61333b153FB:06b0394473ba3adbbc240d0695420a3891b20cfffbb05451959e51632d896e0702d7c86609b47bd08bd12b3d61dc01224c58d30a2bfa4eaee232d639feefdda12b22d23b10fa182f631de7821dfe63a18d521495974e6bf861228bf79918c6bb1252808daf3fce9796ba96742121e6378306eff671efd953f6e60b798657a92c" \
         --validators "/ip4/127.0.0.1/tcp/30302/p2p/16Uiu2HAmEMfC49EUsEPgZFPcY1dW47odWrxWJ3nArNryvK6UBuFq:0x03679198091F9Da027F98598c90e774A2468B901:028af72071e96e2736ca69c0ce7bd33d86e1ef5450797aa2e95a1206dc35b8fe299d563f3cb458103fea30982cc0590ddaa1300c36df09cc32672554af9a670d157e918fb22f8e7232f3e6e6284e7de2cdf8c27f21af103c35fc22e4d8bc8ca90228e32fc39adaea395d37571de2837687db58026cec38de866d0cfd74b5f500" \
         --validators "/ip4/127.0.0.1/tcp/30303/p2p/16Uiu2HAmSRUJcmfVNVMQ2nxStP5JKBktKozJdwfWmpJgHJ4kQZJh:0x5f70789636ec39e589326566E66bf75A177C8a5F:1916960ca41387e77434a301cf548ab0b73ad99ce87eca74bc389a206a1dcf9a079741c11dbd70fed993f78f93f6452d170c31b7de8c92a37a3564c208c92a39200311eba43b767f297ae577e9350adbcd27e04caa4c586c04b07b3bba43fbf52e0ce5b0d22f97b0ab70691b78761335fe84ab15d5fc18464125aea08e2a7b3b" \
         --validators "/ip4/127.0.0.1/tcp/30304/p2p/16Uiu2HAkuv7PL1ug2Gt9rtbUTtXUw3Tn6vTDX6qNVDEWivWAd1CB:0xee38cB7897D0d66Feb40df0FFB93541EC9794Ae2:2d376bde47ff7f8f00e89a689e1fd2cd5489f6513aca214fade3fd36c54f88562e26a3e161bbf9df086e51e6d4dbb52a791ed80b8550ae13dd5a21a6c8dec0532a8acdfa7f88ca3d762c1394d87ee0221fac90fa6053c8135477019f7bb2e50f2d8a269d15e619542632be3ba8844da6b4ca5295ac6c066e71b796647aa7a79a" \
         --consensus polybft \
         --reward-wallet 0x454b08980d36511c47d463b3aad0e9da1672a665:1000000 \
         --transactions-allow-list-admin 0x454b08980d36511c47d463b3aad0e9da1672a665,0x654c50049e2f4bfca4cc12c384e1b5003bbcfcb7 \
         --transactions-allow-list-enabled 0x454b08980d36511c47d463b3aad0e9da1672a665,0x654c50049e2f4bfca4cc12c384e1b5003bbcfcb7 \
         --contract-deployer-allow-list-admin 0x454b08980d36511c47d463b3aad0e9da1672a665,0x654c50049e2f4bfca4cc12c384e1b5003bbcfcb7 \
         --contract-deployer-allow-list-enabled 0x454b08980d36511c47d463b3aad0e9da1672a665,0x654c50049e2f4bfca4cc12c384e1b5003bbcfcb7 \
         --bridge-allow-list-admin  0x454b08980d36511c47d463b3aad0e9da1672a665,0x654c50049e2f4bfca4cc12c384e1b5003bbcfcb7 \
         --bridge-allow-list-enabled 0x454b08980d36511c47d463b3aad0e9da1672a665,0x654c50049e2f4bfca4cc12c384e1b5003bbcfcb7 \
         --premine 0x0:1000000000000000000000000000000 \
         --chain-id 2234 \
         --native-token-config GEENO:GN:18:true:0x454b08980d36511c47d463b3aad0e9da1672a665
     
     ## --transactions-allow-list-admin 
     ## --transactions-allow-list-enabled 
     ## --contract-deployer-allow-list-admin  
     ## --contract-deployer-allow-list-enabled 
     ## --bridge-allow-list-admin 
     ## --bridge-allow-list-enabled  这些地址不能带0x前缀 
     ## --block-gas-limit 数值可以改大一点
     ```

### 配置 Root Chain
  
  1. *部署 StakeManager 合约*
     
     ```shell
     ./polygon-edge polybft stake-manager-deploy \
       --proxy-contracts-admin 0x654c50049e2f4bfca4cc12c384e1b5003bbcfcb7 \
       --private-key $(your private-key) \
       --genesis ./genesis.json \
       --jsonrpc https://rpc-mumbai.maticvigil.com \
       --stake-token 0x537a2E0048ae4d2cabfFfD2Fe125Ad5703Ea6553
       ## --test
     
       ## --proxy-contracts-admin v1.3版本后的更新，该账户地址不能和private-key即部署者地址一样，否则会出现 initialized stake manager contract error!!!
     ```
  
  2. *给 Node 转入 Gas*
     
     为了确保Node能正常部署合约，需要给节点地址转入足够的gas （rootchain 的原生 token），手动转即可。
     
     对于test模式，测试 server 默认已经有的gas token在测试账户中。
     
     ```shell
     ### 仅限test模式
     ./polygon-edge rootchain fund --addresses 0x77C1eedFf656477462ce16084fE5Dc7F8a2507B9 --amounts 1000000000000000000
     ```
     
       也可以使用 rootchain上的 erc-20 作为 gas token，命令如下：
     
     ```shell
     ./polygon-edge rootchain deploy \
       --genesis ./genesis.json \
       --json-rpc http://127.0.0.1:8545 \
       --erc20-token <ERC20_TOKEN_ADDRESS>
     
       ## 替换 <ERC20_TOKEN_ADDRESS> 为 已在rootchain上部署的 erc20 token地址
     ```
3. *部署 Root Chain 合约*
   
   ```shell
   ./polygon-edge rootchain deploy \
     --deployer-key $(your private-key) \
     --stake-manager 0x41ffd39f1D4215A16C2C5A30884387E033bB5cBc \
     --stake-token 0x537a2E0048ae4d2cabfFfD2Fe125Ad5703Ea6553 \
     --proxy-contracts-admin 0x654c50049e2f4bfca4cc12c384e1b5003bbcfcb7 \
     --genesis ./genesis.json \
     --json-rpc https://rpc-mumbai.maticvigil.com
   
     ## --deployer-key 部署者的私钥，默认为本地客户端的默认账户
   ```
### 配置 Validators
  
  1. *在rootchain上设置validators白名单*
     
       在注册 Validator 之前，需要通过 CustomSupernetManager 合约的部署者将 Validator 加入白名单。
     
     ```shell
     ./polygon-edge polybft whitelist-validators \
       --private-key $(your private-key) \
       --addresses 0xf0f63Bf5D51fbbD067CA2595EaC9e61333b153FB,0x03679198091F9Da027F98598c90e774A2468B901,0x5f70789636ec39e589326566E66bf75A177C8a5F,0xee38cB7897D0d66Feb40df0FFB93541EC9794Ae2 \
       --supernet-manager 0xE3EdAFD12a5F45A25181A05ec4119Cb6C49fE2Df \
       --jsonrpc https://rpc-mumbai.maticvigil.com
     
       ## --private-key SupernetManagerProxy合约部署者私钥
       ## --addresses validator 地址
       ## --supernet-manager customsupernetmanager proxy 合约地址，这里是supernetmanager 代理合约地址
     ```
  
  2. *在rootchain上注册validators*
     
     ```shell
     ./polygon-edge polybft register-validator --data-dir ./test-chain-1 \
       --jsonrpc https://rpc-mumbai.maticvigil.com \
       --supernet-manager 0xE3EdAFD12a5F45A25181A05ec4119Cb6C49fE2Df
     
       ####
       ./polygon-edge polybft register-validator --data-dir ./test-chain-2 \
       --jsonrpc https://rpc-mumbai.maticvigil.com \
       --supernet-manager 0xE3EdAFD12a5F45A25181A05ec4119Cb6C49fE2Df
     
       ###
       ./polygon-edge polybft register-validator --data-dir ./test-chain-3 \
       --jsonrpc https://rpc-mumbai.maticvigil.com \
       --supernet-manager 0xE3EdAFD12a5F45A25181A05ec4119Cb6C49fE2Df
     
       ##
         ./polygon-edge polybft register-validator --data-dir ./test-chain-4 \
       --jsonrpc https://rpc-mumbai.maticvigil.com \
       --supernet-manager 0xE3EdAFD12a5F45A25181A05ec4119Cb6C49fE2Df
     ```
     
     输出示例：
     
     ```shell
     [VALIDATOR REGISTRATION]
     Validator Address = 0xf0f63Bf5D51fbbD067CA2595EaC9e61333b153FB
     KOSK Signature    = 2cbfdd31e09a9f94377ba1fef9494332591cee90fb40f3f6b908e76c6de6717b2a9fe8f067c45ddbe36929f560d0526d7de0cfffc2bfdad3d99a407f26ccd406
     ```
  
  3. *在rootchain上初始化质押*
     
       每个 Validator 需要通过 StakeManager 合约来质押token，*注意确保每个Validator地址有足够的stake token*
     
     ```shell
     ./polygon-edge polybft stake \
       --data-dir ./test-chain-1 \
       --supernet-id $(cat genesis.json | jq -r '.params.engine.polybft.supernetID') \
       --amount 1000000000000000000 \
       --stake-manager $(cat genesis.json | jq -r '.params.engine.polybft.bridge.stakeManagerAddr') \
       --stake-token $(cat genesis.json | jq -r '.params.engine.polybft.bridge.stakeTokenAddr') \
     
       ## --stake-token 质押token的地址，即rootchain上erc20的地址
       ## --stake-manager stakemanager合约地址
       ## --amount 质押金额
       ## --supernet-id 自增+1
       ## --data-dir polygon edge data目录
     
       ####### node 1 #######
       ./polygon-edge polybft stake \
       --data-dir ./test-chain-1 \
       --supernet-id 1 \
       --amount 100000000000000000 \
       --stake-manager 0x41ffd39f1D4215A16C2C5A30884387E033bB5cBc \
       --stake-token 0x537a2E0048ae4d2cabfFfD2Fe125Ad5703Ea6553 \
       --jsonrpc https://rpc-mumbai.maticvigil.com
     
       ####### node 2 #######
        ./polygon-edge polybft stake \
       --data-dir ./test-chain-2 \
       --supernet-id 1 \
       --amount 2000000000000000000 \
       --stake-manager 0x41ffd39f1D4215A16C2C5A30884387E033bB5cBc \
       --stake-token 0x537a2E0048ae4d2cabfFfD2Fe125Ad5703Ea6553 \
       --jsonrpc https://rpc-mumbai.maticvigil.com
     
        ####### node 3 #######
        ./polygon-edge polybft stake \
       --data-dir ./test-chain-3 \
       --supernet-id 1 \
       --amount 3000000000000000000 \
       --stake-manager 0x41ffd39f1D4215A16C2C5A30884387E033bB5cBc \
       --stake-token 0x537a2E0048ae4d2cabfFfD2Fe125Ad5703Ea6553 \
       --jsonrpc https://rpc-mumbai.maticvigil.com
     
        ####### node 4 #######
        ./polygon-edge polybft stake \
       --data-dir ./test-chain-4 \
       --supernet-id 1 \
       --amount 4000000000000000000 \
       --stake-manager 0x41ffd39f1D4215A16C2C5A30884387E033bB5cBc \
       --stake-token 0x537a2E0048ae4d2cabfFfD2Fe125Ad5703Ea6553 \
       --jsonrpc https://rpc-mumbai.maticvigil.com
     ```
     
     > Supernet 也允许使用rootchain的原生token作为 stake token，需要将原生token先wrap为erc-20token，如 使用polygon pos 的原生代币 MATIC ，则stake-token地址为 WMATIC的地址
  
  4. *在rootchain上确定最终validators*
     
     ```shell
     ./polygon-edge polybft supernet \
       --private-key $(your private-key) \
       --genesis ./genesis.json \
       --supernet-manager 0xE3EdAFD12a5F45A25181A05ec4119Cb6C49fE2Df \
       --stake-manager 0x41ffd39f1D4215A16C2C5A30884387E033bB5cBc \
       --finalize-genesis-set --enable-staking \
       --jsonrpc https://rpc-mumbai.maticvigil.com
     
       ## --private-key supernetmanager 合约部署者的私钥
       ## --genesis genesis 文件路径
       ## --supernet-manager supernetmanager合约地址
       ## --stake-manager stake-manager合约地址
       ## --finalize-genesis-set 
       ## --enable-staking
     ```

### 启动 Chlid Chain Cluster
  
  1. *启动*
     
       运行前，需要将genesis.json 文件中的 rewardTokenAddress 地址替换为 stake token的真实地址.
     
     ```shell
     ./polygon-edge server --data-dir ./test-chain-1 --chain genesis.json --grpc-address :5001 --libp2p :30301 --jsonrpc :10001 --seal --log-level DEBUG
     
     ./polygon-edge server --data-dir ./test-chain-2 --chain genesis.json --grpc-address :5002 --libp2p :30302 --jsonrpc :10002 --seal --log-level DEBUG
     
     ./polygon-edge server --data-dir ./test-chain-3 --chain genesis.json --grpc-address :5003 --libp2p :30303 --jsonrpc :10003 --seal --log-level DEBUG
     
     ./polygon-edge server --data-dir ./test-chain-4 --chain genesis.json --grpc-address :5004 --libp2p :30304 --jsonrpc :10004 --seal --log-level DEBUG
     ```
2. *测试*
   
   ```shell
   curl  http://127.0.0.1:10001 -X POST -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_chainId","params":[],"id":1}'
   ```
   
   查询交易
   
   ```shell
   curl  http://127.0.0.1:10001 -X POST -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_getBalance","params":["0x454b08980d36511c47d463b3aad0e9da1672a665", "latest"],"id":1}'
   ```
   
   查询余额
   
   ```shell
   curl  http://127.0.0.1:10001 -X POST -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_getTransactionByHash","params":["0x5b5721dd69f1e2b0dd8e66a0babad1b3393f02e19e5b00562db81a2c2c9fa485"],"id":1}'
   ```

## 使用Supernet

### 添加账户白名单

### 转账/部署/调用

### 跨链

## 参考来源

[Polygon官网](https://wiki.polygon.technology/docs/edge/operate/deploy/)
