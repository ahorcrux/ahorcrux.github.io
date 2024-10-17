---
title: 网络 | 使用 acme.sh 自动签发 ssl 证书
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

### 安装 acme.sh

+ https://get.acme.sh 安装（推荐）
  
  ```shell
  curl  https://get.acme.sh | sh

  source ~/.bashrc

  acme.sh --register-account -m youremail@example.com


  ```

  ```shell
  ./acme.sh --install  \
  --home ~/myacme \
  --config-home ~/myacme/data \
  --cert-home  ~/mycerts \
  --accountemail  "my@example.com" \
  --accountkey  ~/myaccount.key \
  --accountconf ~/myaccount.conf \
  --useragent  "this is my client."
  ```
  + --home is a customized dir to install acme.sh in. By default, it installs into ~/.acme.sh
  + --config-home is a writable folder, acme.sh will write all the files(including cert/keys, configs) there. By default, it's in --home
  + --cert-home is a customized dir to save the certs you issue. By default, it's saved in --config-home
  + --accountemail is the email used to register an account to Let's Encrypt, you will receive a renewal notice email here



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

### 注册 acme 客户端



## 签发并验证证书

acme.sh 支持 rsa 、ecc 证书类型。

acme.sh 实现了 acme 协议支持的所有验证协议. 一般有两种方式验证: http 和 dns 验证。

acem.sh 可以签发单域名（-d xx.domain.com）、多域名(-d xx1.domain.com -d xx2.domain.com)、泛域名（-d *.domain.com）证书，还可以签发 ECC 证书。

> 泛域名（*.domain.com）是不包含（domain.com）的，需要再配置 -d domain.com
{: .prompt-warning }

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

  如果你还没有运行任何 web 服务, 80 端口是空闲的, 那么 acme.sh 还能假装自己是一个webserver, 临时听在80 端口, 完成验证。
  ```shell
  acme.sh  --issue -d mydomain.com   --standalone
  ```

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
    > 需要注意的是 如果重复输入这行代码，并不会执行覆盖操作。即只会保存第一次保存的key，如果需要修改需要到 ~/.acme.sh/目录下 修改 account.conf 文件
    {: .prompt-warning }

    ```shell
    ./acme.sh --issue --dns dns_namesilo -d example.com -d *.example.com --debug --dnssleep 900
    ```

    Namesilo DNS解析较慢，建议 --dnssleep 900 改成 1800

解析完成之后, 重新生成证书：

    ```shell
    acme.sh --renew -d mydomain.com \
    --yes-I-know-dns-manual-mode-enough-go-ahead-please
    ```

证书的解释：

cert.pem	服务端证书
chain.pem	浏览器需要的所有证书但不包括服务端证书，比如根证书和中间证书
fullchain.pem	包括了cert.pem和chain.pem的内容
privkey.pem	证书的私钥

官方介绍，Let’s encrypt 提供的 fullchain.pem 文件，其实是把 cert.pem 和 chain.pem 文件粘贴到了一起。如果 cert.pem 出于某种原因不被认可，那么 chain.pem 文件就可以解围。因此在 ssl_certificate 的配置中使用 fullchain.pem 确实更为合适

## Copy 或安装证书

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

+ Trojan

  ```shell
  acme.sh --installcert -d your_domain.com --fullchain-file /path/to/fullchain.crt --key-file /path/to/privkey.key --ecc --force
  ```
  

Nginx 的配置 ssl_certificate 使用 /etc/nginx/ssl/fullchain.cer ，而非 /etc/nginx/ssl/<domain>.cer ，否则 SSL Labs 的测试会报 Chain issues Incomplete 错误。
--install-cert命令可以携带很多参数, 来指定目标文件. 并且可以指定 reloadcmd, 当证书更新以后, reloadcmd会被自动调用,让服务器生效

[详细参数请参考](https://github.com/Neilpang/acme.sh#3-install-the-issued-cert-to-apachenginx-etc):

## 查看证书信息

```shell
acme.sh --info -d example.com
```

## 更新证书

目前证书在 60 天以后会自动更新, 你无需任何操作. 今后有可能会缩短这个时间, 不过都是自动的, 你不用关心。
请确保 cronjob 正确安装：

```shell
crontab  -l
```
看起来是类似这样的：

```shell
56 * * * * "/root/.acme.sh"/acme.sh --cron --home "/root/.acme.sh" > /dev/null
```

如果配置了 trojan 等服务，需要重启服务的话可定时任务加入重启 trojan 命令

```shell
crontab -e
sudo systemctl restart trojan
```

## 手动更新证书

```shell
./acme.sh --renew -d "域名" --force --ecc
```

## 更新 acme.sh

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

## 查看日志

在对应的命令里加入参数 --debug 即可
```shell
acme.sh --issue  .....  --debug 
```

或者

```shell
acme.sh --issue  .....  --debug  2
```

## 简化版

```shell
curl  https://get.acme.sh | sh

source ~/.bashrc

acme.sh --register-account -m youremail@example.com

export Namesilo_Key="your namesilo api key"

acme.sh --issue --dns dns_namesilo -d aaa.example.com --debug --dnssleep 1800

acme.sh --renew -d aaa.example.com

acme.sh --installcert -d aaa.example.com --fullchain-file /path/to/fullchain.crt --key-file /path/to/privkey.key --ecc --force

```

trojan配置

```json
"cert": "/path/to/fullchain.crt",
"key": "/path/to/privkey.key",
```

> 如果 `sudo systemctl start trojan` 启动trojan失败，通过 `journalctl -u trojan -f` 命令发现是 /path/to/privkey.key root没有权限，通过 `chmod 664 privkey.key` 修改权限，重启trojan即可。
{: .prompt-warning }



## 应用案例

### Web 服务80端口跳转443

```shell
server {
    listen 80;
    server_name www.域名.com;
    rewrite ^(.*)$ https://${server_name}$1 permanent; 
}
server {
    listen 443;
    server_name www.域名.com;
    root /home/wwwroot;
    ssl on;
    ssl_certificate /etc/nginx/certs/server.crt;
    ssl_certificate_key /etc/nginx/certs/server.key;
    ....
}

```

### Trojan 服务

trojan 和 nginx 共存

# *参考来源*

+ [acmesh-official](https://github.com/acmesh-official/acme.sh#3-install-the-issued-cert-to-apachenginx-etc)
+ [使用acme.sh签发证书](https://itlanyan.com/use-acme-sh-get-free-cert/)
+ [使用acme.sh免费申请HTTPS证书](https://github.com/acmesh-official/acme.sh/wiki/%E8%AF%B4%E6%98%8E)
+ [使用ACME申请泛域名证书](https://www.panyanbin.com/article/e212b974.html)
+ [namesilo 通过 acme.sh 申请泛域名证书](https://blog.weifengx.com/2021/08/16/namesilo-%E9%80%9A%E8%BF%87-acme-sh-%E7%94%B3%E8%AF%B7%E6%B3%9B%E5%9F%9F%E5%90%8D%E8%AF%81%E4%B9%A6/)
+ [acme.sh DNS验证证书Demo](https://github.com/acmesh-official/acme.sh/wiki/dnsapi)
+ [知乎-Let's Encrypt 安装配置教程](https://zhuanlan.zhihu.com/p/196638669)
+ [知乎-Certbot-免费的HTTPS证书](https://zhuanlan.zhihu.com/p/80909555)
+ [Let's Encrypt 使用教程](https://diamondfsd.com/lets-encrytp-hand-https/)
+ [nginx-acme配置](https://wsgzao.github.io/post/acme/)



