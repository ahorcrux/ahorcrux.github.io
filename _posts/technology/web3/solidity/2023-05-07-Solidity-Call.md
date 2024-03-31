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
https://blastscan.io/address/0x6c1acd1834d049ddb68318e0756e025a6271ccc5#code

```js
// SPDX-License-Identifier: GPL-2.0-or-later
pragma solidity 0.8.24;

/// @title Multicall interface
/// @notice Enables calling multiple methods in a single call to the contract
interface IMulticall {
    /// @notice Call multiple functions in the current contract and return the data from all of them if they all succeed
    /// @dev The `msg.value` should not be trusted for any method callable from multicall.
    /// @param data The encoded function data for each of the calls to make to this contract
    /// @return results The results from each of the calls passed in via data
    function multicall(bytes[] calldata data) external payable returns (bytes[] memory results);
}
```

```js
// SPDX-License-Identifier: GPL-3.0
pragma solidity 0.8.24;

import "../external/uniswap/interfaces/IMulticall.sol";

/// @title Multicall
/// @notice Enables calling multiple methods in a single call to the contract
abstract contract Multicall is IMulticall {
    /// @inheritdoc IMulticall
    function multicall(bytes[] calldata data) public payable virtual override returns (bytes[] memory results) {
        results = new bytes[](data.length);
        for (uint256 i = 0; i < data.length; i++) {
            (bool success, bytes memory result) = address(this).delegatecall(data[i]);

            if (!success) {
                // Next 5 lines from https://ethereum.stackexchange.com/a/83577
                if (result.length < 68) revert();
                assembly {
                    result := add(result, 0x04)
                }
                revert(abi.decode(result, (string)));
            }

            results[i] = result;
        }
    }
}
```

## 常见问题

## 常见攻击

## *参考链接*

+ [WTF Solidity极简入门: 55. 多重调用](https://github.com/AmazingAng/WTF-Solidity/blob/main/55_MultiCall/readme.md)
+ [合约的multicall](https://clark-cui.top/posts/%E5%90%88%E7%BA%A6%E7%9A%84multicall.html)
+ [知乎 - 使用Multicall 加速 DeFi查询调用](https://zhuanlan.zhihu.com/p/356502478)
+ [Youtube - Multi Call合约](https://www.youtube.com/watch?v=U0Wlw6fjY5c)