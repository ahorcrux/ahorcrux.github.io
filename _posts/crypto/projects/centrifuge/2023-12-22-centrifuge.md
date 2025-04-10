---
title: RWA 项目 | Centrifuge 解读
author: since2014
date: 2023-12-22 14:02:00 +0800
categories: [Web3, Project]
tags: [blockchain, web3, crypto, rwa, centrifuge]
render_with_liquid: false
---

## 项目概括

### 简介

[Centrifuge](https://app.centrifuge.io/) 是一种基础设施，可促进现实世界资产在链上的去中心化融资，创建一个完全透明的市场，允许借款人和贷方在没有不必要的中介的情况下进行交易。资产池是完全抵押的，流动性提供者拥有法律追索权，并且该协议与资产类别无关，资产池涵盖抵押贷款、发票、小额贷款和消费金融。

Centrifuge 旨在解决中小企业在缺乏开放和透明的市场中无法获得有竞争力的借款利率的问题，将融资过程编码到区块链中，可以大大减少对这些中介机构的依赖，从而实现更加开放、透明和高效的融资渠道。

### 团队


Centrifuge成立于2017年，由Lucas Vogelsang、Maex Ament和Martin Quensel联合创立。Linkin显示目前团队拥有56人。


## 业务模式

![centrifuge-structure](/img/crypto/p_centrifuge_a_4_legal-structure.png)

### 产品组件

+ [Tinlake](https://legacy.tinlake.centrifuge.io/)
  
  Centrifuge面向C端用户的前端产品，Centrifuge正式推出了新的安全、可靠、简介的升级版App，正在逐渐淘汰Tinlake。

  Tinlake采用分级池， 使放贷者可以投资两个不同的级别：优先级发行一种称为 DROP 的代币，劣后级发行一种称为 TIN 的代币。优先级的较低 / 稳定的收益率，且承担的风险较小，而劣后级的收益率较高 / 波动性更大的收益，并且有更高的贷款违约风险，以此形成对优先级的保护。此结构类似于金融中的常见 A/B 或优先级 / 劣后级结构。
  ![centrifuge-pool](/img/crypto/p_centrifuge_a_7_pool_level.png)

+ [Apps](https://app.centrifuge.io/)

    今年5月，Centrifuge逐步弃用Tinlake ，改用 Apps。

+ [Bridge](https://bridge.centrifuge.io/)

+ Chain

  该链基于Substrate框架开发，能够共享Polkadot网络的安全性。该链将供允许与任何通用EVM区块链直接集成的连接器。CFG为网络原生代币，可桥接到Ethereum。

+ Centrifuge Prime

  Centrifuge 今年 6 月份还宣布还推出了 RWA 基础设施产品 Centrifuge Prime，它主要面向 DAO 和 DeFi 协议打通真实世界资产的法律框架和引资产上链的技术全套服务。Centrifuge Prime是由Centrifuge开发的一套全面的服务和技术，可帮助DeFi原生组织（例如DAO、稳定币和协议）轻松加入和扩展现实世界资产 (RWA) 的投资组合。目前处于测试阶段。

### 产品角色

+ 借款人 (Borrower)
  决定以一些现实世界资产作为抵押物进行融资，这些资产可以是发票或是房产；

+ 资产发起方 (Asset Originator，如 BlockTower)
  
  创建一个特殊目的实体（SPV，作为发行方，issuer），作为每个资金池的独立法律主体，保持每一个资金池的独立；Centrifuge 的合约会在以太坊上创建对应的资金池，并与对应的 SPV 关联起来；

+ 发行人 (SPV)

  Special Purpose Vehicle是一种特殊独立法律实体，职能一般是在离岸资产证券化过程中，购买、包装证券化资产和以此为基础发行资产化证券，主要用于投资。由于业务范围被严格地限定，所以属于一般不会破产的高信用等级实体。

+ 投资人 (Investor)
  投资人指的是向 Centrifuge 资金池提供流动性的参与方。投资人需要完成 KYC 后才能进行交易；

+ 合规方
  合规主要指 Centrifuge 与第三方合作机构开展的 KYC 和 AML 工作。这包括投资人实名和身份信息认证，以及反洗钱监测。当前 Centrifuge 主要通过与证券发行合规公司 Securitize 的合作来完成相关流程。

### 法律结构

每个池都有一个与之相关的法人实体，即特殊目的工具（SPV）。SPV 将资产发起人的业务与池中的融资活动分开，并确保池中的资产远离资产发起人破产。运营协议定义了 SPV 的运营。资产证券化时，资产的法定所有权由资产发起人转移给SPV。

### 产品流程
https://docs.centrifuge.io/learn/legal-offering/
1. 借款人希望通过发票或房地产等资产为抵押在链上进行融资。

2. 资产发起人（与借款人有业务关系并执行承销，也投资于风险更高的 TIN 代币）为资金池设立法人实体SPV。一个资金池可能包括同种类型但对应不同借款人的多笔抵押品贷款。

3. 资产发起人发起并验证RWA，为抵押资产在链上铸造一个 NFT。

4. 借款人与SPV签订融资协议，并要求资产发起人将其 NFT 锁定在与 SPV 绑定的 Centrifuge 池中，Centrifuge 池铸造 DROP 和 TIN 两种代币。

5. Centrifuge 和 SEC 许可的合格投资者验证服务商 Securitize 合作，帮助投资者完成 KYC 和 AML 流程。

6. 投资者与 Centrifuge 池对应的 SPV 签订投资协议，协议中包含了投资结构、风险、条款等，之后用 DAI 购买 DROP 或 TIN 代币。

7. 当有投资者为对应的资金池提供 DAI 的流动性时，SPV 将 DAI 兑换为美元并向借款人的银行账户转账 或者 直接将 DAI 转给借款人。

8. 投资者可以随时要求赎回他们的 DROP 或 TIN 代币，但须保证 DROP 代币优于 TIN 代币赎回，且 TIN 代币不能低于设定的最低比例。

9. 借款人在 NFT 到期时支付融资金额和融资费用，NFT 被返还给资产发起人。

https://web3caff.com/zh/archives/76670

New Silver 系列：投资于房地产过桥贷款的； 
BlockTower 系列：投资于结构性信贷的；
Harbor Trade Credit系列：基于应收账款借贷的  

https://app.centrifuge.io/pools/0x55d86d51Ac3bcAB7ab7d2124931FbA106c8b60c7/assets

其中 BlockTower Series 4 资金池 由资产发起方创建，每个资金池对应一个独立的法律主体SPV, 下方列表为作为链上抵押品的 NFT，每个 NFT 对应一笔由资产发起方验证过的链下现实资产，在该案例中为结构化信贷

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
[Github 地址](https://github.com/centrifuge/tinlake)

### Liquidity-pools 

Centrifuge Chain 的流动性池，可以在任何与 EVM 兼容的区块链上无缝部署 Centrifuge RWA 池。
![centrifuge-pool 合约架构](/img/crypto/p_centrifuge_a_10_lp_pool.png)
[Github 地址](https://github.com/centrifuge/liquidity-pools)

### Bridge

[CFG<>wCFG 桥](https://bridge.centrifuge.io/)是可信中继网桥,由一组受到信任的中继器，在以太坊和 Centrifuge 链之间中继消息。

### Centrifuge Chain

Centrifuge Chain在2022年1月成功赢得了Polkadot的第8次插槽拍卖

到5月17日为止，Centrifuge的主要产品Tinlake还部署在以太坊主网上

基于 Substrate 框架开发，能够共享 Polkadot 网络的安全性，并且已桥接到以太坊

+ Substrate

  ![centrifuge-substrate](/img/crypto/p_centrifuge_a_9_substrate.png)

+ Chain Stack

  ![centrifuge-chain](/img/crypto/p_centrifuge_a_8_chain_stack.png)

  BABE-区块生成/授权

  GRANDPA-区块终结性工具

  NPoS-验证人选举算法

+ 浏览器

  https://centrifuge.subscan.io/

+ 测试网
  Flint、Amber
  后续迁移至 Layer2 网络 Arbitrum 和 Base。

### Centrifuge P2P

它可以让合作者之间安全且隐私地创建、交换和验证资产数据

Paper Records 等公司使用 Centrifuge P2P 网络将发票签名并发送给 Spotify。Spotify 通过该签名来验证文件的接收及正确性，并将文件更新、签名版本发送回 Paper Records。Centrifuge 链用于节点身份，允许Paper Records 查找 Spotify，Spotify 验证 Paper Recrods。然后，Paper Records 就可以将带有两个签名的文档 Hash 锚定到 Centrifuge 链上。使用这些元素，Paper Records 现在可以在 Centrifuge 链上创造一个表示未付发票的 NFT，并将此 NFT 用作抵押品，以获得以太坊等其他区块链上的融资

### POD

Centrifuge NFT 包含定价、融资和估值所需的最重要信息，并且可以锁定到 Centrifuge 协议的池中，作为用于融资的抵押品的代表。然而，默认情况下，这些 NFT 附加的任何链上信息都将是公开的。在现实世界的用例中，投资者通常需要访问有关资产的广泛信息，但发行人通常不会公开这些信息。

![centrifuge-pod](/img/crypto/p_centrifuge_a_11_pod_tokenization.png)

Centrifuge 私有数据层 POD (Centrifuge P2P 节点) 解决了这个问题，允许发行人和投资者安全、私密地访问额外的资产数据。

![centrifuge-pod](/img/crypto/p_centrifuge_a_13_pod_architecture.png)

Centrifuge POD 提供了一个简单的 API 接口来与 p2p 网络和 Centrifuge Chain 进行交互。POD 在“服务总线”主体上运行，其中插件和外部系统可以订阅有关特定对象的消息（例如，采购应用程序可以订阅订单对象的更改）。POD 抽象公共区块链和 P2P 层上发生的事件，并将其转换为内部总线上的消息，供其他应用程序使用

https://docs.centrifuge.io/getting-started/privacy-first-tokenization/

https://docs.centrifuge.io/build/nfts/

# *参考链接*
+ https://docs.centrifuge.io/
+ https://www.gate.io/zh/learn/articles/what-is-centrifuge/752
+ https://foresightnews.pro/article/detail/48310
+ https://m.mytokencap.com/news/283536
+ https://medium.com/bsos-taiwan/centrifuge-chain-ethereum-d1e22f1b2178
+ https://www.chaincatcher.com/article/2062026
+ https://news.marsbit.co/20230518165414358768.html
+ https://medium.com/bsos-taiwan/centrifuge-chain-ethereum-d1e22f1b2178

