---
title: 
author: since2014
date: 2023-10-28 18:02:00 +0800
categories: [Web3, Project]
tags: [blockchain, web3, crypto, rwa]
render_with_liquid: false
---


资产发起方（asset originator，如 BlockTower）创建一个特殊目的实体（SPV，作为发行方，issuer），作为每个资金池的独立法律主体，保持每一个资金池的独立；Centrifuge 的合约会在以太坊上创建对应的资金池，并与对应的 SPV 关联起来；
借款人（borrower）决定以一些现实世界资产作为抵押物进行融资，这些资产可以是发票或是房产；
资产发起方发行该抵押物对应的 RWA 代币，进行验证，并铸造对应的 NFT 作为链上抵押品；
借款人和 SPV 就融资条款达成协议；资产发起方将 NFT 锁定在于 SPV 相关的 Centrifuge 资金池中，从该池的储备中提取稳定币 Dai；Dai 可以直接转入借款人的钱包，或由 SPV 转换为美元进入借款人对应的银行账户；
借款人在 NFT 到期日偿还融资金额和费用，可以选择链上以 Dai 直接还款，或是通过银行转账，再通过 SPV 转换为 Dai 支付给 Centrifuge 资金池；一旦所有 NFT 完全偿还，他将解锁并归还给资产发起方，可以被销毁。


https://app.centrifuge.io/pools/0x55d86d51Ac3bcAB7ab7d2124931FbA106c8b60c7/assets

其中 BlockTower Series 4 资金池 由资产发起方创建，每个资金池对应一个独立的法律主体SPV, 下方列表为作为链上抵押品的 NFT，每个 NFT 对应一笔由资产发起方验证过的链下现实资产，在该案例中为结构化信贷

## *参考链接*

