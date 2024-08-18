---
title: Vpn | Trojan 客户端之 Clash
author: since2014
date: 2021-11-25 14:44:00 +0800
categories: [Technology, Network, Vpn]
tags: [vpn, 科学上网, trojan, clash]
render_with_liquid: false
---



## 介绍

## 下载

[官网下载](https://www.clashforwindows.net/clash-for-windows-download/)

## 安装

下载后，点击 exe 安装

## 设置

### 修改配置

配置分为订阅地址和 `Config` 文件

+ Config

    [下载配置模板文件](https://v2xtls.org/clash_template2.yaml), 打开文件并找到 troajn 配置块，修改 `server` 、`port` 、`password` 为自己的服务器配置信息。

    ```shell
    - name: "trojan"
        type: trojan
        server: www.xxx.com 
        port: 443 
        password: abc123 
        # udp: true
        # sni: example.com 
        alpn:
            - h2
            - http/1.1
        # skip-cert-verify: true
    ```

    | 字段        | 说明          |
    | :--------- | :------------ |
    | `server`   | 你的服务器地址 |
    | `port`     | 你的服务器端口 |
    | `password` | 你的密码       |

    > 如果server填的ip地址，请取消sni的注释(删除sni前面的空格和#号)，填上trojan的域名，或者取消skip-cert-verify的注释
    {: .prompt-warning }
    
    
+ 订阅地址


## 高级配置

### 修改节点配置

+ 如果只有单个代理节点，找到对应类型修改成你服务端的信息就行了
  
+ 如果想删除多余的节点，把 `-name` 开头的整个节点配置删除；
  
+ 如果想修改节点名称，比如把 ”ss” 改成 “香港-ss节点” ，改name后面的值；
  
+ 如果想增加节点，根据节点类型(ss/vmess/trojan)复制节点模板，粘贴到节点列表下面，修改 `name` 和 `server` 等配置信息的值
  
  > 需要注意的是，V2ray节点的 `type` 是 `vmess` ，不是 `v2ray` ！
  {:.prompt-warning }

> 修改了节点名称、增加或者删除了节点，其他关联的地方也需要修改 !
{:.prompt-warning }

### Proxies 模式

+ `Global`
  
  全局模式，即所有连接都走代理

+ `Rule`
  
  配置规则代理，根据规则命中连接代理

+ `Direct` 
  
  直连，即所有连接不走代理

选择 rule 之后，有些网址不能访问，tradingview.com ？
# *参考来源*

+ [Trojan-Wins客户端-clash配置](https://v2xtls.org/clash-for-windows%e9%85%8d%e7%bd%aetrojan%e6%95%99%e7%a8%8b/)
+ [深入理解clash配置](https://v2xtls.org/%e6%b7%b1%e5%85%a5%e7%90%86%e8%a7%a3clash%e9%85%8d%e7%bd%ae%e6%96%87%e4%bb%b6/)


