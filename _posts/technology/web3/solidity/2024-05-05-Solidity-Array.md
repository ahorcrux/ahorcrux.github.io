---
title: 
author: since2014
date: 2023-05-07 14:32:33 +0800
categories: [Technology, Blockchain]
tags: [blockchain, web3, solidity]
render_with_liquid: false
---

## 语法

## 案例

## 常见问题

## 常见攻击

## 源码

```js
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.14;

contract AbiUtil {

    function decode(bytes memory data) public pure returns (uint256 p1) {
        (p1) = abi.decode(data, (uint256));
    }
}
```

# *参考链接*

+ [whatsweb3-solidity](https://www.whatsweb3.org/docs/solidity-basic/array)