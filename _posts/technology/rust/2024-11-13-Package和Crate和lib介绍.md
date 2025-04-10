---
title: Rust | 包、Crate和模块管理
author: since2014
date: 2024-11-13 15:22:00 +0800
categories: [Technology, Rust]
tags: [Rust, Cargo]
render_with_liquid: false
---

# 概念

Rust中，Package 可以理解为项目，Crate 可以理解为 包

## Package 项目

可以理解为java中的 Project 或 Project Module。包含独立的 Cargo.toml 文件。

可以分为 二进制 Package 和 库文件 Package

### Package 二进制

可执行的包，有一个主函数 main.rs 入口

### Package 库

Library 类型的包，

## Crate 包

包是一个独立的可编译单元，它编译后会生成一个可执行文件或者一个库。

一个包会将相关联的功能打包在一起，使得该功能可以很方便的在多个项目中分享。

例如标准库中没有提供但是在三方库中提供的 rand 包，它提供了随机数生成的功能，我们只需要将该包通过 use rand; 引入到当前项目的作用域中，就可以在项目中使用 rand 的功能：rand::XXX。


# 参考来源

+ [包和 Package](https://course.rs/basic/crate-module/crate.html)
+ [使用包、Crate 和模块管理不断增长的项目](https://kaisery.github.io/trpl-zh-cn/ch07-00-managing-growing-projects-with-packages-crates-and-modules.html)