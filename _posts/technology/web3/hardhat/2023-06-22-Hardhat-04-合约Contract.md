---
title: Hardhat 教程 | 常用代码示例
author: since2014
date: 2023-06-22 11:16:00 +0800
categories: [Technology, Blockchain]
tags: [blockchain, web3, hardhat, 使用技巧]
render_with_liquid: false
---

# Provider

# 合约工厂 Factory

`Ether.js` 创造了 `ContractFactory` 类，方便开发人员部署合约。

## 实例化

```ts
const factory = new ethers.ContractFactory(abi, bytecode, wallet);
```

+ `abi` : 



+ `bytecode` :
+ `wallet` : 

## Api

### 部署合约

通过 `.deploy()` 函数实现合约部署。

```ts
const contract = await factory.deploy(param1, param2);
```

`.deploy(param1, param2)` 中的参数 `param` ,`param2` 用于传递给合约的构造函数，如果没有则不填。



# 合约 Contract

### 获取客户端provider实例

+ 自定义配置网络
  
  在你的hardhat配置文件中添加自定义网络，比如你自己的本地ganache，或者是公开的以太坊测试网络/主网络。
  
  ```js
  // hardhat.config.js
  module.exports = {
    networks: {
      rinkeby: {
        url: "https://eth-rinkeby.alchemyapi.io/v2/your-alchemy-key",
        accounts: [`0x${yourPrivateKey}`],
      },
    },
  };
  ```
  
  然后在你的脚本中，你可以使用`hre.ethers.provider`来连接到这个网络:
  
  ```js
  const hre = require("hardhat");
  const ethers = hre.ethers;
  ```

+ 直接连接到公开网络
  
  ```js
   // 你的合约地址和ABI
    const contractAddress = "0x70BaD09280FD342D02fe64119779BC1f0791BAC2";
    const contractAbi = ["function sendMessage(address, uint256, bytes) public"];
  
    // 连接到以太坊并创建一个新的签名者
    const provider = hre.ethers.getDefaultProvider("https://rpc.ankr.com/eth_goerli"); // 这里为本地节点，如果是主网或测试网，替换成相应的RPC URL
    const wallet = new hre.ethers.Wallet(PRIVATE_KEY, provider);
  
    // 创建一个新的合约实例
    const contract = new hre.ethers.Contract(contractAddress, contractAbi, wallet);
  
     // 设置转账数量，这里以0.01个ETH为例
     const amount = hre.ethers.parseEther('0.01');
  
     // 准备需要传递的参数，这里你需要替换为你的实际参数
     const address = "0xfd16B08Fd907BA8E1aC9A63FEbA02e9A8e9F1b27";
     const uint256Value = 1000000000000000;
     const bytesValue = "0x";
  
    // sendMessage
    const tx = await contract.sendMessage(address, uint256Value, bytesValue, {value : amount});
    await tx.wait();
    console.log("Number set successful.");
  ```

### 获取合约实例

+ 通过合约地址和Abi
  ```js
  const contract = new ethers.Contract(contractAddress, contractAbi, provider);
  ```

+ 通过合约工厂
  
   如果你正在使用Hardhat，并且你的合约已经被编译，你可以使用`getContractFactory`方法来获取一个合约工厂，然后使用这个工厂来部署一个新的合约或者连接到一个现有的合约
  
  ```js
  const factory = await ethers.getContractFactory("YourContract");
  const contract = await factory.deploy(); // 部署一个新的合约
  // 或者
  const contract = factory.connect(contractAddress); // 连接到一个现有的合约
  ```

+ connect 与 attach
  
  你有一个合约工厂（ContractFactory）实例，并且你想要使用一个特定的签名者（Signer）与合约交互时，你可以使用 `connect()` 方法。这个方法需要一个 `Signer` 作为参数，它会返回一个新的合约实例，这个实例使用**指定的 `Signer` 来发送交易**
  
  ```js
  const factory = await ethers.getContractFactory("YourContract");
  const signer = provider.getSigner();
  const contract = factory.connect(signer).attach(contractAddress);
  ```
  
  ```js
  const factory = await ethers.getContractFactory("YourContract");
  const contract = factory.attach(contractAddress);
  
  // or 
  
  const contract = new ethers.ContractFactory(contractAbi, bytecode).attach(contractAddress);
  ```


当你知道一个合约的地址，并且你想要获取这个合约的实例时，你可以使用 `attach()` 方法。这个方法需要一个合约地址作为参数，它会返回一个新的合约实例。

connect方法主要用来改变合约示例的签名者，而attach()方法主要用来获取一个已经部署的合约的示例。

### 调用链上已存在合约

+ **存在合约源文件**



```js
const hre = require("hardhat");

async function main() {
  // 获取合约实例
  const contractAddress = "0x537a2E0048ae4d2cabfFfD2Fe125Ad5703Ea6553";
  const ERC20 = await hre.ethers.getContractFactory("YourErc20ContractName");
  const contract = ERC20.attach(contractAddress);

  // 要mint的地址和数量
  const toAddress = "0xYourAddress";
  const amount = ethers.utils.parseUnits("200", "18"); // 假设你的token有18位小数

  // 调用mint方法
  const tx = await contract.mint(toAddress, amount);
  await tx.wait();

  console.log(`Minted 200 tokens to ${toAddress}`);
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```

+ **无合约源文件**

```js
const hre = require("hardhat");
const ethers = hre.ethers;

async function main() {
  // 你的合约地址和ABI
  const contractAddress = "0xYourContractAddress";
  const contractABI = [
    // 这里插入你的ABI
  ];

  // 获取合约实例
  const contract = new ethers.Contract(contractAddress, contractABI, ethers.provider);

  // 调用合约的mint函数
  const toAddress = "0xYourAddress";
  const amount = ethers.utils.parseUnits("200", "18"); // 假设你的token有18位小数
  const tx = await contract.mint(toAddress, amount);
  await tx.wait();

  console.log(`Minted 200 tokens to ${toAddress}`);
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```

## 跨链

## 参考来源

+ https://haofly.net/hardhat/