---
title: 网络 | 使用 CloudFront 申请免费 SSL 证书
author: since2014
date: 2024-10-18 15:44:00 +0800
categories: [Technology, Network]
tags: [cloudfront]
render_with_liquid: false
---

# 介绍

使用 Cloudflare 配置 SSL 证书非常简单，只需要通过 Namesilo 更改 DNS 服务器到 Cloudflare 提供的地址，并在 Cloudflare 中启用 SSL/TLS 设置。这个过程不需要你手动生成或安装证书，Cloudflare 会为你自动处理 SSL 证书的生成和续期。

# 安装

## 注册 Cloudflare 账号

1. 访问 Cloudflare 网站：前往 Cloudflare 的官网。
2. 创建账号：点击右上角的 “Sign Up” 按钮，使用你的电子邮件地址注册一个 Cloudflare 账号。

## 添加域名到 Cloudflare


1. 登录 Cloudflare 后，在仪表板中选择 “Add a Site”（添加站点）。
2. 输入你的域名：在弹出的输入框中输入你购买的域名，例如 abc.com。
3. 选择计划：Cloudflare 会让你选择一个服务计划。你可以选择 Free Plan（免费计划），足够满足大部分 SSL 和 CDN 需求。
4. Cloudflare 开始扫描 DNS 记录：Cloudflare 会扫描你当前的 DNS 记录并列出它们。你需要确保这些 DNS 记录正确无误。通常，Cloudflare 会自动获取所有的现有 DNS 记录。


## 更新 Namesilo DNS 设置

1. 查看 Cloudflare 提供的 DNS 服务器地址：Cloudflare 会生成两组名称服务器（NS 记录），你需要将你的域名的 NS 记录指向 Cloudflare 提供的这些地址。
2. 登录到 Namesilo 管理后台：前往 Namesilo 并登录到你的账户。
3. 管理 DNS 设置：
  + 在 Namesilo 中找到你域名的设置页面，选择 Manage My Domains。
  + 选择你要设置的域名，例如 abc.com。
  + 在域名管理页面中，找到 Change Nameservers 或者类似的选项。
  + 将 Cloudflare 提供的名称服务器替换现有的名称服务器，保存设置。
  + 等待生效：DNS 变更可能需要 24-48 小时才能完全生效，但通常只需要几个小时。

## 配置 SSL 设置

1. 回到 Cloudflare，选择你的域名。

2. 导航到 “SSL/TLS” 选项卡：在 Cloudflare 仪表板的左侧菜单中，找到并点击 “SSL/TLS”。

3. 选择 SSL 模式：

+ 默认情况下，Cloudflare 会启用 SSL 并选择 "Flexible" 模式。这种模式适合初次设置，但建议使用 "Full" 或 "Full (strict)" 模式，前提是你的服务器本身也支持 SSL 证书。
Flexible：用户和 Cloudflare 之间的连接使用 HTTPS，但 Cloudflare 和你的服务器之间使用 HTTP。
+ Full：用户和 Cloudflare 之间以及 Cloudflare 和你的服务器之间都使用 HTTPS，服务器需要安装自签名证书。
+ Full (strict)：同样是 HTTPS，但服务器需要安装受信任的 SSL 证书。

4. 启用 Always Use HTTPS：

在 SSL/TLS 设置页面中，找到并启用 “Always Use HTTPS”。这样确保所有请求都会强制使用 HTTPS。


## 配置 Page Rules（可选）

如果你想为某些特定页面或路径设置更高级的 SSL 行为，可以使用 Cloudflare 的 “Page Rules” 功能。这对高级用户和定制化需求非常有用

## 等待 SSL 证书生效

Cloudflare 可能需要几分钟到几个小时来为你的域名生成并配置 SSL 证书。你可以在 Cloudflare 仪表板的 “SSL/TLS” 页面中检查证书状态。当证书生成完成后，你的域名将可以通过 HTTPS 访问。

## 结合 Trojan 注意事项

其中 local_addr 和 local_port 是向外提供服务 (即翻墙) 的地址. 由于我们将使用 Cloudflare 的 CDN 服务, 所以如果你想修改 local_port 你只能从 443, 2053, 2083, 2087, 2096, 8443 这些端口中选择. 这是由于 Cloudflare 对端口的限制

https://helium7.me/posts/trojan-go-ws-cdn/

# *参考来源*





