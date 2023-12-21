---
title: 
author: since2014
date: 2023-10-28 18:02:00 +0800
categories: [Web3, Project]
tags: [blockchain, web3, crypto, rwa]
render_with_liquid: false
---

## 介绍
https://app.centrifuge.io/

Centrifuge成立于2017年，由Lucas Vogelsang、Maex Ament和Martin Quensel联合创立。显示目前团队拥有56人

信贷龙头





https://app.centrifuge.io/pools/0x55d86d51Ac3bcAB7ab7d2124931FbA106c8b60c7/assets

其中 BlockTower Series 4 资金池 由资产发起方创建，每个资金池对应一个独立的法律主体SPV, 下方列表为作为链上抵押品的 NFT，每个 NFT 对应一笔由资产发起方验证过的链下现实资产，在该案例中为结构化信贷


## 业务模式

![centrifuge-structure](/img/crypto/p_centrifuge_a_4_legal-structure.png)

### 产品组件

+ [Tinlake](https://legacy.tinlake.centrifuge.io/)
  
  Centrifuge面向C端用户的前端产品，Centrifuge正式推出了新的安全、可靠、简介的升级版App，正在逐渐淘汰Tinlake。

  Tinlake采用分级池， 使放贷者可以投资两个不同的级别：优先级发行一种称为 DROP 的代币，劣后级发行一种称为 TIN 的代币。优先级的较低 / 稳定的收益率，且承担的风险较小，而劣后级的收益率较高 / 波动性更大的收益，并且有更高的贷款违约风险，以此形成对优先级的保护。此结构类似于金融中的常见 A/B 或优先级 / 劣后级结构。
  ![centrifuge-pool](/img/crypto/p_centrifuge_a_7_pool_level.png)

+ [Apps](https://app.centrifuge.io/)

+ [Bridge](https://bridge.centrifuge.io/)

+ Chain

  该链基于Substrate框架开发，能够共享Polkadot网络的安全性。该链将供允许与任何通用EVM区块链直接集成的连接器。CFG为网络原生代币，可桥接到Ethereum。

+ Centrifuge Prime

  Centrifuge 今年 6 月份还宣布还推出了 RWA 基础设施产品 Centrifuge Prime，它主要面向 DAO 和 DeFi 协议打通真实世界资产的法律框架和引资产上链的技术全套服务。Centrifuge Prime是由Centrifuge开发的一套全面的服务和技术，可帮助DeFi原生组织（例如DAO、稳定币和协议）轻松加入和扩展现实世界资产 (RWA) 的投资组合。目前处于测试阶段。

### 产品角色

+ 发行人 (SPV)

  Special Purpose Vehicle是一种特殊独立法律实体，职能一般是在离岸资产证券化过程中，购买、包装证券化资产和以此为基础发行资产化证券，主要用于投资。由于业务范围被严格地限定，所以属于一般不会破产的高信用等级实体。

+ 资产发起方 (Asset Originator，如 BlockTower)
  
  创建一个特殊目的实体（SPV，作为发行方，issuer），作为每个资金池的独立法律主体，保持每一个资金池的独立；Centrifuge 的合约会在以太坊上创建对应的资金池，并与对应的 SPV 关联起来；

+ 借款人 (Borrower)
  决定以一些现实世界资产作为抵押物进行融资，这些资产可以是发票或是房产；

+ 投资人 (Investor)
  投资人指的是向 Centrifuge 资金池提供流动性的参与方。投资人需要完成 KYC 后才能进行交易；

+ 合规方
  合规主要指 Centrifuge 与第三方合作机构开展的 KYC 和 AML 工作。这包括投资人实名和身份信息认证，以及反洗钱监测。当前 Centrifuge 主要通过与证券发行合规公司 Securitize 的合作来完成相关流程。

### 法律结构

每个池都有一个与之相关的法人实体，即特殊目的工具（SPV）。SPV 将资产发起人的业务与池中的融资活动分开，并确保池中的资产远离资产发起人破产。运营协议定义了 SPV 的运营。资产证券化时，资产的法定所有权由资产发起人转移给SPV。

### 产品流程
https://docs.centrifuge.io/learn/legal-offering/
1. 借款人希望通过发票或房地产等资产为抵押在链上进行融资。

2. 资产发起人（与借款人有业务关系并执行承销，也投资于风险更高的TIN代币）为资金池设立法人实体SPV。一个资金池可能包括同种类型但对应不同借款人的多笔抵押品贷款。

3. 资产发起人发起并验证RWA，为抵押资产在链上铸造一个NFT。

4. 借款人与SPV签订融资协议，将NFT抵押在Tinlake池中，Tinlake池铸造DROP和TIN两种代币。

5. Centrifuge和SEC许可的合格投资者验证服务商Securitize合作，帮助投资者完成KYC和AML流程。

6. 投资者与Tinlake池对应的SPV签订投资协议，协议中包含了投资结构、风险、条款等，之后用DAI购买DROP或TIN代币。

7. 当有投资者为对应的资金池提供DAI的流动性时，SPV将DAI兑换为美元，向借款人的银行账户转账。

8. 投资者可以随时要求赎回他们的DROP或TIN代币，但须保证DROP代币优于TIN代币赎回，且TIN代币不能低于设定的最低比例。

9. 借款人在NFT到期时支付融资金额和融资费用，NFT被返还给资产发起人。

例子：https://web3caff.com/zh/archives/76670


### 集成DeFi

![centrifuge-defi](/img/crypto/p_centrifuge_a_6_defi.png)
+ MakerDao
  
  Centrifuge上有多个MakerDAO相关的资金池，这些资金池和MakerDAO的Vault集成，可以直接从Vault中提取资金
  BlockTower Series 3、BlockTower Series 4
  优先级资本从Maker Vault获得，次级资本由发起人BlockTower出资
+ Aave
  Aave 今年 8 月份就通过了与 Centrifuge Prime 合作投资美债的提案
  Aave则和Centrifuge共同建立了一个专用于RWA的借贷池。

## 操作指南

https://docs.centrifuge.io/use/setup-wallet/


## 架构设计

![centrifuge架构图](/img/crypto/p_centrifuge_a_5_architectur.png)

### Tinlake Protocol

一组部署在以太坊上的智能合约，它将现实世界资产转换为ERC-20代币，然后提供去中心化借贷协议的访问权限。
  
### Liquidity-pools 

### Bridge

标准化的以太坊桥，可以被其他项目重复使用，从而增加了互操作性

https://bridge.centrifuge.io/



### Centrifuge Chain

Centrifuge Chain在2022年1月成功赢得了Polkadot的第8次插槽拍卖

到5月17日为止，Centrifuge的主要产品Tinlake还部署在以太坊主网上

基于 Substrate 框架开发，能够共享 Polkadot 网络的安全性，并且已桥接到以太坊

+ Chain Stack

  ![centrifuge-pool](/img/crypto/p_centrifuge_a_8_chain_stack.png)

+ Substrate

  ![centrifuge-substrate](/img/crypto/p_centrifuge_a_9_substrate.png)

+ 浏览器

  https://centrifuge.subscan.io/

+ 测试网
  Flint
  Amber

### Centrifuge P2P

它可以让合作者之间安全且隐私地创建、交换和验证资产数据

Paper Records 等公司使用 Centrifuge P2P 网络将发票签名并发送给 Spotify。Spotify 通过该签名来验证文件的接收及正确性，并将文件更新、签名版本发送回 Paper Records。Centrifuge 链用于节点身份，允许Paper Records 查找 Spotify，Spotify 验证 Paper Recrods。然后，Paper Records 就可以将带有两个签名的文档 Hash 锚定到 Centrifuge 链上。使用这些元素，Paper Records 现在可以在 Centrifuge 链上创造一个表示未付发票的 NFT，并将此 NFT 用作抵押品，以获得以太坊等其他区块链上的融资

### Substrate
BABE-区块生成/授权

GRANDPA-区块终结性工具

NPoS-验证人选举算法

### POD

https://docs.centrifuge.io/getting-started/privacy-first-tokenization/

https://docs.centrifuge.io/build/nfts/

## *参考链接*
+ https://docs.centrifuge.io/
+ https://www.gate.io/zh/learn/articles/what-is-centrifuge/752
+ https://foresightnews.pro/article/detail/48310
+ https://m.mytokencap.com/news/283536
+ https://medium.com/bsos-taiwan/centrifuge-chain-ethereum-d1e22f1b2178
+ https://www.chaincatcher.com/article/2062026
+ https://news.marsbit.co/20230518165414358768.html
+ https://medium.com/bsos-taiwan/centrifuge-chain-ethereum-d1e22f1b2178

