---
title: 
author: since2014
date: 2023-05-07 14:32:33 +0800
categories: [Technology, Blockchain]
tags: [blockchain, web3, solidity]
render_with_liquid: false
---


# Call

call和delegatecall都是低级别的函数调用方法，但它们在合约间调用时有重要的区别。

调用合约的上下文：call 会在被调用的合约的上下文中执行，即调用者的上下文信息（例如 msg.sender 和 msg.value）会变成被调用合约的地址和发送的金额。
状态变量：调用合约的状态变量不会影响被调用合约的状态变量，两者之间是独立的

# Delegatecall

调用合约的上下文：delegatecall 会在调用者的上下文中执行，即 msg.sender 和 msg.value 都保留原调用者的信息。
状态变量：调用合约的状态变量会影响调用者的状态变量，它们共享同一个存储

上下文：call 在被调用合约的上下文中执行，而 delegatecall 在调用者的上下文中执行。
状态变量：call 使状态变量在不同合约之间是独立的，而 delegatecall 使状态变量在调用者和被调用合约之间是共享的。

使用 call 的场景：

当你希望调用的合约执行在其自身的上下文中。
当你希望合约之间状态变量是独立的，不会相互影响。

使用 delegatecall 的场景：

当你希望被调用的合约在调用者的上下文中执行，保持原调用者的 msg.sender 和 msg.value。
当你希望被调用合约能够修改调用者的状态变量，这通常用于实现合约的可升级性

# Multi Call

multi_call是Solidity中的一种设计模式，用于在单个交易中调用多个函数。这种方法在需要批量操作时非常有用，因为它可以减少交易次数，从而降低Gas费用，并且在逻辑上将多个操作组合在一起，保证原子性（要么全部成功，要么全部失败）。

## 作用

+ **批量操作**：

允许在一个交易中执行多个函数调用，适用于需要对多个合约或同一合约的多个函数进行批量操作的场景。
+ **降低Gas费用**：

通过减少交易次数，multi_call可以有效降低Gas费用。

+ **原子性**：

保证多个操作在一个交易中原子执行，所有操作要么全部成功，要么全部回滚。

+ **简化用户体验**：

用户只需要发起一个交易即可完成多个操作，简化了操作流程。

## 案例 

### 1

```js
pragma solidity ^0.8.0;

contract MultiCall {
    struct Call {
        address target;
        bytes callData;
    }

    function multiCall(Call[] memory calls) public returns (bytes[] memory results) {
        results = new bytes[](calls.length);
        for (uint256 i = 0; i < calls.length; i++) {
            (bool success, bytes memory result) = calls[i].target.call(calls[i].callData);
            require(success, "Call failed");
            results[i] = result;
        }
    }
}

```

### 2
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

# 常见问题

# 常见攻击

# *参考链接*

+ [WTF Solidity极简入门: 55. 多重调用](https://github.com/AmazingAng/WTF-Solidity/blob/main/55_MultiCall/readme.md)
+ [合约的multicall](https://clark-cui.top/posts/%E5%90%88%E7%BA%A6%E7%9A%84multicall.html)
+ [知乎 - 使用Multicall 加速 DeFi查询调用](https://zhuanlan.zhihu.com/p/356502478)
+ [Youtube - Multi Call合约](https://www.youtube.com/watch?v=U0Wlw6fjY5c)