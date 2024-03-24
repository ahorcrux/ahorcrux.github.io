---
title: Vpn | Trojan 客户端 Clash
author: since2014
date: 2021-11-25 14:44:00 +0800
categories: [Technology, Network, Vpn]
tags: [vpn, 科学上网, trojan, qv2ray]
render_with_liquid: false
---



## 介绍

## 下载

[官网下载](https://www.clashforwindows.net/clash-for-windows-download/)

## 安装

下载后，点击 exe 安装

## 配置

配置分为订阅地址和 config 文件

+ config 文件

    先[下载配置模板文件](https://v2xtls.org/clash_template2.yaml), 打开文件并找到 troajn 配置块，修改 server 、port 、password 为自己的服务器配置信息。

    ```shell
    - name: "trojan"
    type: trojan
    server: www.xxx.com # 修改成你自己的服务器地址
    port: 443 # 修改成你自己的服务器端口
    password: abc123 #修改成你的密码
    # udp: true
    # sni: example.com # 填写伪装域名
    alpn:
        - h2
        - http/1.1
    # skip-cert-verify: true
    ```

    > 如果server填的ip地址，请取消sni的注释(删除sni前面的空格和#号)，填上trojan的域名，或者取消skip-cert-verify的注释
    {: .prompt-warning }
    
+ 订阅地址


## *参考来源*

+ [Trojan-Wins客户端-clash配置](https://v2xtls.org/clash-for-windows%e9%85%8d%e7%bd%aetrojan%e6%95%99%e7%a8%8b/)
+ [深入理解clash配置](https://v2xtls.org/%e6%b7%b1%e5%85%a5%e7%90%86%e8%a7%a3clash%e9%85%8d%e7%bd%ae%e6%96%87%e4%bb%b6/)


