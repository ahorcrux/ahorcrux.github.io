---
title: 
author: since2014
date: 2023-10-28 18:02:00 +0800
categories: [Technology, Blockchain]
tags: [blockchain, web3, crypto]
render_with_liquid: false
---

## 前言

根据波士顿咨询集团的一份报告，预计到2030年,代币化资产市场规模将达到16万亿美元,相比2022年的3100亿美元表现出巨大的增长空间
许多协议已经集成了RWAs或正在参与其增长

## 什么是RWA

现实世界资产(RWA)是指真实世界中的有形（房地产、大宗商品、收藏品）和无形资产（债券、股票、碳信用）。RWA代币化就是将这些链下资产转入区块链中，从而开辟全新可能性和开发潜在用例。代币化RWA可在链上存储和追踪，从而提高效率和透明度，同时降低人为错误的可能性。

## 为什么是RWA

### 传统金融投资的弊端

+ 门槛高
+ 效率低
+ 费用高
+ 透明度低

在Web2或者现实世界中，如果投资证券、股票，对于新手来讲面临的第一个问题就是开户的手续繁琐，审核流程周期长。如果需要投资国外的债券或者股票，又需要经历监管的流程，这个过程中监管机构的运营成本会转化为高额的手续费和合规费。

### RWA的优势


+ 透明与可追溯

    Token 化之后的资产，无论怎么包装，怎么衍生，其中的过程都是透明的，投资者可以追溯其基础资产，在充分了解该资产的风险的基础上进行买卖。DeFi 将比 TradFi 更加透明，交易者可以了解到更加完整的信息，也可以更好的防范类似于“次贷危机”的风险积累和系统性危机。

+ 降低商业摩擦

    在区块链上，交易即结算，购买即确权。因此区块链可以极大的降低交易的成本，提升交易效率。同时区块链能够7x24小时的运行，最大程度的减少中间人的参与，从而消除交易过程中的对手风险。

+ 灵活性与可组合性

    2020年 DeFi Summer 之后, DeFi的交易、借贷等基础设施已经较为完善，借由智能合约的可编程性和Defi的成功经验，链上资产可以轻松的进行拆分、组合，改变资产形态亦或创造新的资产类型。


### RWA为什么最近又火了
![rwa tvl 趋势图](/img/technology/web3_rwa_a_2_tvl.png)
(来源：https://app.rwa.xyz/)

1. 机构入场

    今年，RWA得到了高盛、花旗、币安、MakerDAO等机构和链上头部协议的合力推动

2. Defi 收益率下降，国债类收益上升
    ![rwa-美债](/img/technology/web3_rwa_a_3_treasuries.png)
    2020年Defi Summer牛市期间，高收益率（100%~5000%）的Defi项目吸引大量投资者和和机构涌入，但是随着加密市场进入深熊，DeFi 的整体流动性不足，且 CeFi（3AC、FTX等）出现暴雷潮，Maple 和 TrueFi 为代表的机构借贷协议遭受了大额度违约坏账，随着DeFi创新的减速以及市场的发展，老牌DeFi的收益率已经不满足市场需求。反之，美联储不断加息带来国债收益上升。

## 运作机制

### 结构

1. 真实世界

    资产发起方、资产托管机构、资产购买渠道。  
    链下资产金融化，

2. 信息桥梁

    预言机、法律框架、代币标准、第三方审计、出入金支付渠道。  
    金融数据可信化，

3. 链上部分

    RWA代币发行方、发行平台、智能合约。  
    可信数据代币化，

### 基本流程

![rwa运作流程](/img/technology/web3_rwa_a_01_liucheng.png)

## 生态

### 稳定币 (USD)

市值排名第三的稳定币USDT可以说是RWA领域最成功的项目之一，因为它将美元映射到链上并代币化。

+ USDT
+ USDC

### 美债

![rwa-国债协议占比](/img/technology/web3_rwa_a_4_treasuries_protocol.png)
![rwa-国债协议排名](/img/technology/web3_rwa_a_5_defilama_rank.png)
+ MakerDAO RWA
    6月21日，MakerDAO购买了另外价值7亿美元的美国国债，使其总持有量达到12亿美元。该协议价值23亿美元的RWA抵押品现已成为该协议资产中最大的部分，占Maker资产的49%。
+ Ondo Finance
+ Maple Finance

### 房地产 (Real Estate)

+ RealT
+ LABS Group

### 信贷

+ Goldfinch
+ Centrifuge
+ Clearpoll


### 碳信用 (Carbon Project)

+ Toucan
+ Flowcarbon

### 公链

+ POLYMESH
+ MANTRA
+ Realio Network

### 其他

贵金属、Fintech (金融科技)、Auto (汽车)、TradFi、气候、收藏品等。


### 对比点
    1、KYC、AML和合规性
    2、信用以及信用违约的挽救方案
    3、透明度
    4、流动性


## 挑战

### 身份
### 监管合规
### 风险管理
### 技术风险
常见的智能合约漏洞。

Curvo 以太坊智能合约编程语言Vyper被攻击，在7月31日凌晨Vyper 0.2.15至0.3.0之间版本的防重入锁失效。恶意行为者利用重入攻击反复重新签署合约，导致未经授权的操作或资金被盗。

连锁反应

DeFi 借贷协议 Alchemix、NFT 借贷协议 JPEG'd、DeFi 合成资产协议 MetronomeDAO、跨链桥 deBridge 和采用 Curve 机制的 BNB Chain上DEX 项目 Ellipsis 累计损失超 2676 万美元。

CRV-ETH 被攻击，链上 CRV最低跌到0.08左右，但由于 AAVE 的价格取自 Chainlink，后者并没把异常价格体现，使得 Curve 创始人 Michael Egorov 在 AAVE 的仓位未被清算。

## ERC-3643

Permissioned tokens + digital identity

```js
interface IERC3643 is IERC20 {

   // events
    event UpdatedTokenInformation(string _newName, string _newSymbol, uint8 _newDecimals, string _newVersion, address _newOnchainID);
    event IdentityRegistryAdded(address indexed _identityRegistry);
    event ComplianceAdded(address indexed _compliance);
    event RecoverySuccess(address _lostWallet, address _newWallet, address _investorOnchainID);
    event AddressFrozen(address indexed _userAddress, bool indexed _isFrozen, address indexed _owner);
    event TokensFrozen(address indexed _userAddress, uint256 _amount);
    event TokensUnfrozen(address indexed _userAddress, uint256 _amount);
    event Paused(address _userAddress);
    event Unpaused(address _userAddress);


    // functions
    // getters
    function onchainID() external view returns (address);
    function version() external view returns (string memory);
    function identityRegistry() external view returns (IIdentityRegistry);
    function compliance() external view returns (ICompliance);
    function paused() external view returns (bool);
    function isFrozen(address _userAddress) external view returns (bool);
    function getFrozenTokens(address _userAddress) external view returns (uint256);

    // setters
    function setName(string calldata _name) external;
    function setSymbol(string calldata _symbol) external;
    function setOnchainID(address _onchainID) external;
    function pause() external;
    function unpause() external;
    function setAddressFrozen(address _userAddress, bool _freeze) external;
    function freezePartialTokens(address _userAddress, uint256 _amount) external;
    function unfreezePartialTokens(address _userAddress, uint256 _amount) external;
    function setIdentityRegistry(address _identityRegistry) external;
    function setCompliance(address _compliance) external;

    // transfer actions
    function forcedTransfer(address _from, address _to, uint256 _amount) external returns (bool);
    function mint(address _to, uint256 _amount) external;
    function burn(address _userAddress, uint256 _amount) external;
    function recoveryAddress(address _lostWallet, address _newWallet, address _investorOnchainID) external returns (bool);

    // batch functions
    function batchTransfer(address[] calldata _toList, uint256[] calldata _amounts) external;
    function batchForcedTransfer(address[] calldata _fromList, address[] calldata _toList, uint256[] calldata _amounts) external;
    function batchMint(address[] calldata _toList, uint256[] calldata _amounts) external;
    function batchBurn(address[] calldata _userAddresses, uint256[] calldata _amounts) external;
    function batchSetAddressFrozen(address[] calldata _userAddresses, bool[] calldata _freeze) external;
    function batchFreezePartialTokens(address[] calldata _userAddresses, uint256[] calldata _amounts) external;
    function batchUnfreezePartialTokens(address[] calldata _userAddresses, uint256[] calldata _amounts) external;
}

```
https://eips.ethereum.org/EIPS/eip-3643
https://github.com/ethereum/ercs/blob/master/ERCS/erc-3643.md
https://github.com/ERC-3643/ERC-3643/tree/main

## 参考来源
+ [rwa.xyz](https://www.rwa.xyz/)
+ [Dune](https://dune.com/impossiblefinance/rwa-lending-landscape)
+ https://www.hellobtc.com/kp/du/06/4239.html
+ https://www.chaincatcher.com/special/114
+ https://foresightnews.pro/article/detail/48740
+ https://www.binance.com/zh-CN/blog/ecosystem/%E5%B8%81%E5%AE%89%E7%A0%94%E7%A9%B6%E9%99%A2%E7%8E%B0%E5%AE%9E%E4%B8%96%E7%95%8C%E8%B5%84%E4%BA%A7%E7%9A%84%E7%8E%B0%E7%8A%B6%E7%A0%94%E7%A9%B6-4697414668665991002
+ https://public.bnbstatic.com/static/files/research/real-world-assets-state-of-the-market-cn.pdf
+ https://www.erc3643.org/
+ https://defillama.com/protocols/RWA
+ https://www.youtube.com/watch?v=HExvz9d1JR8