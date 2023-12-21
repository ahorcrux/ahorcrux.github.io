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

Real Estate 房地产
Fintech 金融科技
Auto 汽车
Crypto Trading 加密货币交易投资
Consumer 
Carbon Project 碳项目

## 什么是RWA
现实世界资产(RWA)是指真实世界中的有形（房地产、大宗商品、收藏品）和无形资产（债券、股票、碳信用）。RWA代币化就是将这些链下资产转入区块链中，从而开辟全新可能性和开发潜在用例。代币化RWA可在链上存储和追踪，从而提高效率和透明度，同时降低人为错误的可能性。

USDT USDC
## 为什么需要RWA

传统金融投资的弊端
+ 门槛高
+ 效率低
+ 费用高
+ 透明度低
在Web2或者现实世界中，如果投资证券、股票，对于新手来讲面临的第一个问题就是开户的手续繁琐，审核流程周期长。如果需要投资国外的债券或者股票，又需要经历监管的流程，这个过程中监管机构的运营成本会转化为高额的手续费和合规费。

RWA的优势
+ 基于完善的区块链基础设施，保证安全性和可追溯的同时，提高透明度；
+ 智能合约的自动运行机制能简化手续，大幅提升效率；
+ 借鉴Defi的成功经验，增加流动性、灵活性和可组合性；


### RWA为什么最近又火了
[图tvl]
来源：https://app.rwa.xyz/
2020年Defi Summer牛市期间，高收益率（百分之几百甚至几千）的Defi项目吸引大量投资者和和机构涌入，但是随着加密市场进入深熊，DeFi 的整体流动性不足，且 CeFi（FTX）出现暴雷潮，Maple 和 TrueFi 为代表的机构借贷协议遭受了大额度违约坏账
随着DeFi创新的减速以及市场的发展，老牌DeFi的收益率已经不满足市场需求

https://www.ibitcoin86.com/news/86888.html

降低商业摩擦
灵活性与可组合性
透明与可追溯

## 生态


## 运作机制
结构
真实世界
    资产发起方、资产托管机构、资产购买渠道
信息桥梁
    预言机、法律框架、代币标准、第三方审计、出入金支付渠道
链上部分
    RWA代币发行方、发行平台、智能合约

基本流程




包括美债方向的 MakerDAO、Ondo Finance、Maple Finance，TradFi 方向的 Polytrade、Defactor，借贷方向的 Goldfinch、Centrifuge、Clearpool，公共固定收益方向的 Swarm Markets、Acquire.Fi，房地产方向的 RealT、Tangible、LABS Group，碳信用方向的 Toucan、Flowcarbon、PERL.eco，公链领域的 Polymesh、MANTRA Chain 等。
https://www.shandian.io/detail/19622

## 代表协议

协议分类
    美债类

对比点
    1、KYC、AML和合规性
    2、信用以及信用违约的挽救方案
    3、透明度
    4、流动性

### 公链

+ POLYMESH
+ MANTRA
+ Realio Network
  

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