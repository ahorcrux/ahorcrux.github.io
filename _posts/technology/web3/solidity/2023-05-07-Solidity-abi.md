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

+ [medium-abi-encode-and-decode-using-solidity](https://medium.com/coinmonks/abi-encode-and-decode-using-solidity-2d372a03e110)
+ [WTF Solidity极简入门: 27. ABI编码解码](https://github.com/AmazingAng/WTF-Solidity/blob/main/27_ABIEncode/readme.md)
+ [Abi在线编解码](https://abi.hashex.org/)