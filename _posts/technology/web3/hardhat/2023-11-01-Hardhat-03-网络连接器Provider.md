---
title: Hardhat 教程 (三) | Ethers 使用方法
author: since2014
date: 2023-11-1 20:03:00 +0800
categories: [Technology, Blockchain]
tags: [blockchain, web3, hardhat, ether.js]
render_with_liquid: false
---

## Providers

Provider是一个连接到以太坊网络的抽象类，提供只读形式来*查询网络状态*，读取智能合约，获取账户余额等等，还可以*监听网络中的事件*。但是，它*不能执行修改状态的操作*，比如发送交易，因为它没有私钥。

### 派生类&实例化

+ **API Providers**
  
  通过第三方服务与节点通信。

  + EtherscanProvider
    EtherscanProvider 是基于 Etherscan 服务的 Provider 。
    可以使用标准网络名称为参数实例化："homestead" 、"rinkeby" 、"ropsten" 、"kovan" 。
    也可以使用 ChainId 为参数实例化，如下：
    ```ts
     // 连接到主网 (homestead)
     provider = new EtherscanProvider();

     // 连接到 Goerli 测试网 (两中方式效果一样)
     provider = new EtherscanProvider("goerli");
     provider = new EtherscanProvider(5);

     network = ethers.providers.getNetwork("goerli");
     // {
     //   chainId: 5,
     //   ensAddress: '0x00000000000C2E074eC69A0dFb2997BA6C7d2e1e',
     //   name: 'goerli'
     // }

     provider = new EtherscanProvider(network);

     // 连接到主网，配置APIKEY
     provider = new EtherscanProvider(null, apiKey);
     provider = new EtherscanProvider("homestead", apiKey);
    ```
  + InfuraProvider
    InfuraProvider 是基于 Infura 服务的 Provider 。
  + AlchemyProvider
    AlchemyProvider 是基于 Alchemy 服务的 Provider 。
  + CloudflareProvider
    CloudflareProvider 是基于 Cloudflare 服务的 Provider 。


+ **DefaultProvider**
  DefaultProvider 同时链接 Etherscan 和 Infura 等多个不同的后端服务，如果某个服务出现问题，会自动切换到其他服务。ethers.getDefaultProvider()返回的是一个FallbackProvider实例，这是对多个网络提供商的封装，如果你不提供任何参数，它将默认连接到所有这些网络提供商，并在它们之间进行负载均衡和失败切换。适用高可用和健壮性强的场景。

  
  ```ts
  const defaultProvider = ethers.getDefaultProvider('ropsten');
  ```
  
  通过Url连接到指定节点，该方式不会自动切换其他服务，失去了高可用特点。
  ```ts
  const defaultProvider = ethers.getDefaultProvider('[ropsten](https://eth-sepolia.public.blastapi.io)');
  ```

+ **JsonRpcProvider**
  
  JsonRpcProvider 基于 JSON-RPC 协议的 Provider，适用于支持这种协议的以太坊节点交互。

  ```ts
  const provider = new ethers.providers.JsonRpcProvider("http://127.0.0.1:8545");
  ```
  ```ts
  const INFURA_ID = '';
  const providerETH = new ethers.providers.JsonRpcProvider(`https://mainnet.infura.io/v3/${INFURA_ID}`);
  ```

  返回一个 JsonRpcProvider 的实例，这个实例只会连接到你指定的节点，不会进行负载均衡或失败切换。如果你指定的节点发生故障，你需要手动更换节点。然而。

  + JsonRpcSigner
  + JsonRpcUncheckedSigner
  
+ **其他Provider**
  + Web3Provider
    基于 Web3.js 的 Provider，通过Web3.js与节点通信，适用于在浏览器环境中使用，可以与 Metamask 等浏览器插件交互。
    
    连接Metamask
    ```ts
    const provider = new ethers.providers.Web3Provider(window.etherum);
    ```
  + WebSocketProvider

    ```ts
    const provider = new ethers.providers.WebSocketProvider("wss://bsc-ws-node.nariox.org:443");
    ```

  + IpcProvider

    ```ts
    const path = "/var/run/parity.ipc";
    const ipcProvider = new ethers.providers.IpcProvider(path);
    ```

  + FallbackProvider

    通过依次尝试每个 提供者Provider 来提高可靠性，如果遇到错误，则返回列表中的下一个提供程序。 网络由 提供者Provider 确定，必须 相互匹配。

    ```ts
    let provider = new ethers.providers.FallbackProvider([
       new ethers.providers.InfuraProvider(network),
       new ethers.providers.EtherscanProvider(network),
       new ethers.providers.JsonRpcProvider('http://localhost:8545'),
    ]);
    ```

  + UrlJsonRpcProvider
  
  
### 属性&API
https://docs.ethers.org/v5/single-page/#/v5/api/providers/
+ Block - 返回指定高度的区块信息
+ Block - 返回指定高度的区块信息(带交易列表)
+ Account - 返回账户余额
+ Account - 返回账户交易数量
+ Account - 返回指定区块高度下账户的合约代码
+ ENS - 返回ens对应头像的url
+ ENS - 返回Address对应的ENS
+ ENS - 返回ENS对应的Address
+ Logs - 
+ Network
+ Transaction
+ Event

> Provider只能提供读取链上数据的方法。如果我们想要向链上写入数据，我们需要让合约实例连接到 Signer。

## Signers

在ethers中Signer是以太坊账户的抽象，包含了私钥的对象，封装了和签名相关的API，可以用来签名消息和交易，如将签名的交易发送到以太坊网络以执行状态更改的操作。
Signer内部包含了一个Provider，用来执行查询操作。
Signer类是抽象的，不能直接实例化，而是应该使用一个具体的子类，如 Wallet, VoidSigner 或 JsonRpcSigner。


### Wallet

+ New Instance
  助记词
     ```ts
     const mnemonic = "announce room limb pattern dry unit scale effort smooth jazz weasel alcohol"
     const walletMnemonic = Wallet.fromMnemonic(mnemonic)
     ```

  私钥实例化，privateKey创建一个新的钱包实例，并可选地连接到provider
     ```ts
     walletPrivateKey = new Wallet(walletMnemonic.privateKey, provider)；
     ```

     ```ts
     privateKey1 = '0xxxxxxx';
     wallet = new ethers.Wallet(privateKey1);
     signer = wallet.connect(provider);
     ```
  新建
     ```ts
     let wallet = ethers.Wallet.createRandom();
     let signer = wallet.connect(provider);
     ```

+ API

### JsonRpcSigner

## Contract

### 属性

+ address：返回合约地址或 ENS 名称
+ deployTransaction：如果合约通过ContractFactory部署，返回部署的交易，否则为null 
+ interface：

### 实例化

+ ContractFactory创建新合约
  
  ```ts
  
  const factory = await ethers.getContractFactory("SwapUniV3");
  factory.connect(signer);
  ```
  ```ts

  const abi = [
    "event ValueChanged(address indexed author, string oldValue, string newValue)",
    "constructor(string value)",
    "function getValue() view returns (string value)",
    "function setValue(string value)"];
  const bytecode = "60806040526012600560006101000a81548160ff021916908360ff1602179055503480156xxxxxx";
  const factory = new ethers.ContractFactory(abi, bytecode, wallet);

  ```
+ 连接已有合约
  
  ```ts
  // The Contract interface
  let abi = [
  "event ValueChanged(address indexed author, string oldValue, string newValue)",
  "constructor(string value)",
  "function getValue() view returns (string value)",
  "function setValue(string value)"
  ];
  // Connect to the network
  let provider = ethers.getDefaultProvider();
  // 地址来自上面部署的合约
  let contractAddress = "0x2bD9aAa2953F988153c8629926D22A6a5F69b14E";
  // 使用Provider 连接合约，将只有对合约的可读权限
  let contract = new ethers.Contract(contractAddress, abi, provider);

  //改变新的signer
  const txSigner= tokenContract.connect(signer);
  ```

  ```ts
  //合约源文件
  const token2Factory = await ethers.getContractFactory('WrappedEther');
  const token2Contract = token2Factory.attach(token2_WETH);

  ```
  
  ```ts
  // ethers.getContractAt函数的作用是创建一个合约实例, 
  // 该函数需要两个参数：
  // 1.合约的ABI（Application Binary Interface），这是一个JavaScript数组，描述了合约的函数和事件。在上述示例中，我们传入了字符串"ERC20"，这是因为ethers.js内置了ERC20标准的ABI。
  // 2.合约的地址。这应该是已经部署的智能合约的Ethereum地址。
  const WETHContract = await ethers.getContractAt("ERC20", token2_WETH);

  ```
+ 

### 部署

  ```ts
  //等待部署
  const erc20 = await erc20Contract.deploy(args1,args2...);
  //获取合约地址
  const erc20Address = await erc20.getAddress();
  //获取部署合约的交易
  console.log(contract.deployTransaction.hash);
  
  //等待合约部署
  //1.合约还没有部署;我们必须等到它被挖出
  await contract.deployed();

  //2.在交易被挖出来之前(即合约被部署)等待（Wait）
  //  - 返回receipt
  //  - throws on failure (reciept发生错误)
  await contract.deployTransaction.wait()
  // 现在合约时可以用来交互了
  await contract.value()
  ```

### 调用合约

+ 只读
  
     ```ts
     // 获取当前的值
     let currentValue = await contract.getValue();
     console.log(currentValue);
     ```

+ 修改
  
     ```ts
     // 从私钥获取一个签名器 Signer
     let privateKey = '0x0123456789012345678901234567890123456789012345678901234567890123';
     let wallet = new ethers.Wallet(privateKey, provider);

     // 使用签名器创建一个新的合约实例，它允许使用可更新状态的方法
     let contractWithSigner = contract.connect(wallet);
     // ... 或 ...
     // let contractWithSigner = new Contract(contractAddress, abi, wallet)

     // 设置一个新值，返回交易
     let tx = await contractWithSigner.setValue("I like turtles.");

     // 查看: https://ropsten.etherscan.io/tx/0xaf0068dcf728afa5accd02172867627da4e6f946dfb8174a7be31f01b11d5364
     console.log(tx.hash);
     // "0xaf0068dcf728afa5accd02172867627da4e6f946dfb8174a7be31f01b11d5364"

     // 操作还没完成，需要等待挖矿
     await tx.wait();

     // 再次调用合约的 getValue()
     let newValue = await contract.getValue();

     console.log(currentValue);
     // "I like turtles."
     ```

+ 

Contract实例化时，是选择provider还是signer?
https://learnblockchain.cn/docs/ethers.js/api-contract.html#providers-signers

https://docs.ethers.org/v5/single-page/

```js
// 只读; 通过 Provider连接，允许以下操作:
// - 访问常量函数 constant function
// - 查询过滤器
// - 对于 non-constant methods 填充未签名的交易
// - 对于non-constant估算Gas(作为匿名发送者)
// - 对于non-constant methods静态调用 (作为匿名发送者)
const erc20 = new ethers.Contract(address, abi, provider);

// 可读写; 通过Signer连接，允许以下操作:
// - 任何只读的操作 (作为Signer而非匿名者)
// - 为non-constant functions发送交易
const erc20_rw = new ethers.Contract(address, abi, signer);

//先provider，providerOrSigner是不可变的，如果需要更改其他account or provider，可以通过connect实现
 const tokenContract = new ethers.Contract(contractAddress, tokenAbi, connection);
var signer = new ethers.Wallet(privateKey, connection);
const txSigner= tokenContract.connect(signer);
const transaction = await txSigner.transfer(to,address,amount)
```

## 参考来源

+ [登链社区 - Ether.js v5 中文文档](https://learnblockchain.cn/ethers_v5/)
+ [登链社区 - Eether.js中文文档](https://learnblockchain.cn/docs/ethers.js/getting-started.html)
+ [知乎 - 使用 React 和 ethers.js 构建DApp](https://zhuanlan.zhihu.com/p/564934900)
+ [ethers.org](https://docs.ethers.org/v5/single-page/#/v5/api/providers/provider/-%23-BaseProvider)
+ [WTF-Ethers](https://github.com/WTFAcademy/WTF-Ethers/blob/main/02_Provider/readme.md)
+ [Mirror - xyyme](https://mirror.xyz/xyyme.eth/hT78s8EI3wYp-IixUC951fQNWTON-EhNcGd2jXEtcdM)