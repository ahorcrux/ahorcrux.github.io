---
title: 网络 | CloudFront 介绍
author: since2014
date: 2024-03-18 15:44:00 +0800
categories: [Technology, Network]
tags: [cloudfront]
render_with_liquid: false
---

## 介绍

Let's Encrypt 是一个由非营利性组织 互联网安全研究小组（ISRG）提供的免费、自动化和开放的证书颁发机构（CA）。

简单的说，借助 Let's Encrypt 颁发的证书可以为我们的网站免费启用 HTTPS(SSL/TLS) 。

Let's Encrypt免费证书的签发/续签都是脚本自动化的，官方提供了几种证书的申请方式方法，点击此处 快速浏览。



这里直接使用第三方客户端 acme.sh 申请，据了解这种方式可能是目前 Let's Encrypt 免费证书客户端最简单、最智能的 shell 脚本，可以自动发布和续订 Let's Encrypt 中的免费证书。

## 安装

### 客户端工具

+ Certbot
  
    官方推荐使用 Certbot 客户端来签发证书，Certbot是使用Python编写的，需要处理Python环境的配置，可参考[官方文档](https://eff-certbot.readthedocs.io/en/latest/)自行尝试。

+ Acme.sh
  
    acme.sh是一个基于Shell脚本的工具，它在各种Linux系统上运行，并且没有其他依赖关系。更加轻量级和易于安装和使用。

    建议国内小伙伴使用aeme.sh，中文友好、自动化程度更高。

### 安装Acem.sh

+ https://get.acme.sh 安装（推荐）
  
  ```shell
  curl  https://get.acme.sh | sh
  source ~/.bashrc
  acme.sh --register-account -m youremail@example.com
  ```

+ GitHub 安装
  
  ```shell
    git clone https://github.com.cnpmjs.org/acmesh-official/acme.sh.git
    cd acme.sh
    ./acme.sh --install -m my@example.com
  ```

+ 第三方安装
  
  ```shell
  curl https://raw.githubusercontent.com/acmesh-official/acme.sh/master/acme.sh | sh -s -- --install-online -m my@example.com
  ```

安装脚本会执行以下几个步骤：

1. 将 acme.sh 安装到 home 目录下
   

2. 创建一个 bash 的 alias
   
   ```shell
   alias acme.sh=~/.acme.sh/acme.sh
   ```

3. 创建 cronjob，每天 0:00 自动检测所有证书，如果快过期了，会自动更新证书

### 生成并验证证书

acme.sh 实现了 acme 协议支持的所有验证协议. 一般有两种方式验证: http 和 dns 验证.

+ Http
  
  http 方式需要在你的网站根目录下放置一个文件, 来验证你的域名所有权,完成验证. 然后生成证书。

  ```shell
  acme.sh --issue -d mydomain.com -d www.mydomain.com --webroot /home/wwwroot/mydomain.com/
  ```
  
  只需要指定域名, 并指定域名所在的网站根目录. acme.sh 会全自动的生成验证文件, 并放到网站的根目录, 然后自动完成验证. 最后会自动删除验证文件。

  如果你用的 apache 服务器, acme.sh 可以智能的从 apache的配置中自动完成验证, 不需要指定网站根目录：

  ```shell
  acme.sh --issue -d mydomain.com --apache
  ```

  同样，nginx 服务器, 或者反向代理, acme.sh 可以智能的从 nginx的配置中自动完成验证, 不需要指定网站根目录：

  ```shell
  acme.sh --issue -d mydomain.com --nginx
  ```
  
  > 无论是 apache 还是 nginx 模式, acme.sh在完成验证之后, 会恢复到之前的状态, 都不会私自更改你本身的配置. 好处是你不用担心配置被搞坏, 也有一个缺点, 你需要自己配置 ssl 的配置, 否则只能成功生成证书, 你的网站还是无法访问https. 但是为了安全, 建议自己手动改配置
  {: .prompt-warning }


+ Dns
  
  1. 手动 dns 方式
   
    手动在域名上添加一条 txt 解析记录, 验证域名所有权。

    这种方式的好处是, 你不需要任何服务器, 不需要任何公网 ip, 只需要 dns 的解析记录即可完成验证. 坏处是，如果不同时配置 Automatic DNS API，使用这种方式 acme.sh 将无法自动更新证书，每次都需要手动再次重新解析验证域名所有权。

    ```shell
    acme.sh --issue --dns -d mydomain.com \ 
    --yes-I-know-dns-manual-mode-enough-go-ahead-please
    ```

    acme.sh 会生成相应的解析记录显示出来, 你只需要在你的域名管理面板中添加这条 txt 记录即可。

  2. 自动Dns 方式

    Dns 方式的真正强大之处在于可以使用域名解析商提供的 api 自动添加 txt 记录完成验证.acme.sh 目前支持 cloudflare, dnspod, cloudxns, godaddy 以及 ovh 等数十种解析商的自动集成.以 dnspod 为例, 你需要先登录到 dnspod 账号, 生成你的 api id 和 api key, 都是免费的. 然后：

    ```shell
    export DP_Id="1234"
    export DP_Key="sADDsdasdgdsf"
    acme.sh --issue --dns dns_dp -d aa.com -d www.aa.com
    ```

    证书就会自动生成了. 这里给出的 api id 和 api key 会被自动记录下来, 将来你在使用 dnspod api 的时候, 就不需要再次指定：

    ```shell
    acme.sh --issue -d mydomain2.com --dns  dns_dp
    ```

    附带 [Namesilo案例](https://github.com/acmesh-official/acme.sh/wiki/dnsapi#dns_namesilo) ：
    ```shell
    export Namesilo_Key="<key>"
    ```
    ```shell
    ./acme.sh --issue --dns dns_namesilo -d example.com -d *.example.com --debug --dnssleep 900
    ```

    Namesilo DNS解析较慢，建议 --dnssleep 900 改成 1800

解析完成之后, 重新生成证书：

    ```shell
    acme.sh --renew -d mydomain.com \
    --yes-I-know-dns-manual-mode-enough-go-ahead-please
    ```

### 安装证书

证书生成以后, 接下来需要把证书 copy 到真正需要用它的地方：

注意, 默认生成的证书都放在安装目录下: ~/.acme.sh/, 请不要直接使用此目录下的文件, 例如: 不要直接让 nginx/apache 的配置文件使用这下面的文件. 这里面的文件都是内部使用, 而且目录结构可能会变化.

正确的使用方法是使用 --install-cert 命令,并指定目标位置, 然后证书文件会被copy到相应的位置, 例如：

+ Apache
  
  ```shell
  acme.sh --install-cert -d example.com \
  --cert-file      /path/to/certfile/in/apache/cert.pem  \
  --key-file       /path/to/keyfile/in/apache/key.pem  \
  --fullchain-file /path/to/fullchain/certfile/apache/fullchain.pem \
  --reloadcmd     "service apache2 force-reload"
  ```

+ Nginx
  
  ```shell
  acme.sh --install-cert -d example.com \
  --key-file       /path/to/keyfile/in/nginx/key.pem  \
  --fullchain-file /path/to/fullchain/nginx/cert.pem \
  --reloadcmd     "service nginx force-reload"
  ```

> 这里用的是 service nginx force-reload, 不是 service nginx reload, 据测试, reload 并不会重新加载证书, 所以用的 force-reload。
{: .prompt-warning }

Nginx 的配置 ssl_certificate 使用 /etc/nginx/ssl/fullchain.cer ，而非 /etc/nginx/ssl/<domain>.cer ，否则 SSL Labs 的测试会报 Chain issues Incomplete 错误。
--install-cert命令可以携带很多参数, 来指定目标文件. 并且可以指定 reloadcmd, 当证书更新以后, reloadcmd会被自动调用,让服务器生效

[详细参数请参考](https://github.com/Neilpang/acme.sh#3-install-the-issued-cert-to-apachenginx-etc):

### 查看证书信息

```shell
acme.sh --info -d example.com
```

### 更新证书

目前证书在 60 天以后会自动更新, 你无需任何操作. 今后有可能会缩短这个时间, 不过都是自动的, 你不用关心。
请确保 cronjob 正确安装：

```shell
crontab  -l
```
看起来是类似这样的：

```shell
56 * * * * "/root/.acme.sh"/acme.sh --cron --home "/root/.acme.sh" > /dev/null
```

### 更新 acme.sh

手动升级 acme.sh 到最新版：

```shell
acme.sh --upgrade
```

如果不想手动升级，开启自动升级:

```shell
acme.sh --upgrade --auto-upgrade
```

关闭自动升级：

```shell
acme.sh --upgrade --auto-upgrade  0
```

### 查看日志

在对应的命令里加入参数 --debug 即可
```shell
acme.sh --issue  .....  --debug 
```

或者

```shell
acme.sh --issue  .....  --debug  2
```

## *参考来源*

+ [使用acme.sh签发证书](https://itlanyan.com/use-acme-sh-get-free-cert/)
+ [使用acme.sh免费申请HTTPS证书](https://github.com/acmesh-official/acme.sh/wiki/%E8%AF%B4%E6%98%8E)
+ [使用ACME申请泛域名证书](https://www.panyanbin.com/article/e212b974.html)
+ [acme.sh DNS验证证书Demo](https://github.com/acmesh-official/acme.sh/wiki/dnsapi)
+ [知乎-Let's Encrypt 安装配置教程](https://zhuanlan.zhihu.com/p/196638669)
+ [知乎-Certbot-免费的HTTPS证书](https://zhuanlan.zhihu.com/p/80909555)
+ [Let's Encrypt 使用教程](https://diamondfsd.com/lets-encrytp-hand-https/)



