---
id: hackathon-startup-guide
title: Hackathon Startup Guide
sidebar_label: Hackathon Guide
---

欢迎使用NEAR！ 很高兴在这里遇见你。让我们直接开始吧：

1) 首先第一件事，当然是获取一个NEAR [`testnet` 账号](https://wallet.testnet.near.org)。如果你在这个过程中遇到了任何困难，请查看这个 [快速指南](/docs/develop/basics/create-account) 解决问题。

2) 现在你已经有了账号，可以尝试一下最简单的 `testnet` 应用并与区块链进行交互， [Berry Club](https://test.berryclub.io/) 或者 [Guest Book](https://near-examples.github.io/guest-book/) 都可以。

3) 试一试 [NEAR Explorer ](https://explorer.testnet.near.org)。在这里你可以查找所有NEAR上产生的交易和区块。你也可以试试查找你刚创建的账号，并查看你刚使用 Berry Club 或 Guest Book 发起的交易。

4) 现在安装 [`near-cli`](/docs/tools/near-cli#setup)。这是一个能让你与NEAR交互的命令行工具。[这个页面](/docs/tools/near-cli) 已经列出了所有的 `near-cli` 命令及相关示例。

5) 试试运行你的第一条命令：[`near login`](/docs/tools/near-cli#near-login)。这条命令将重定向到你的 NEAR 钱包，并将你的 `testnet` 账号及密钥保存在本地。_可以在你的 HOME 目录下看到它们存储在 (~/.near-credentials) 这个隐藏文件里_

6) 试试 `create-near-app` 命令! 在你的终端中运行 `npx create-near-app your-awesome-project` 。 _(请注意：这里需要预先安装 [Node.js](https://nodejs.org/en/)。)_ 这是启动一个区块链上全栈应用的最简单快捷的方法，只需要不到5分钟。

7) 前往 [NEAR 示例](https://near.dev) ，尝试一下示例应用程序。你可以复制这些代码并随便玩玩看，或者就直接点击 Gitpod 按钮启动一个在线的实例！

8) 准备好开始了吗?? 可以再了解一下这些样板应用以及配套的视频教程 [ [这里](https://github.com/near-apps/ ) ]。


## 了解智能合约

智能合约就是你的应用的后端，它存储于区块链中。你的应用仍然需要一些你习惯使用的前端实现 (比如HTML/CSS/JS) ，但是所有的数据, 或者 "state"(状态)，都会存储于 _区块链中_ 。

- 智能合约在区块链网络中运行代码、存储数据。
- 前端实现通过 API (JSON RPC Interface) 与智能合约交互。
- `near-api-js` 是我们为你准备的一个与 NEAR 交互的 JavaScript 库。
  
- 目前，我们支持使用以下几种开发智能合约的语言：
  - [Rust](https://www.rust-lang.org/)
  - [AssemblyScript](https://assemblyscript.org/introduction.html)


## 常见问题

### 1. 从前端向合约发送数据

假设你已经在智能合约中定义了一个读取数据的 Rust 函数：

```ts
pub fn some_method(&mut self, my_data: String) {
    [...]
}
```

当你在前端调用它时，你将很难发送数据,就像下面这个错误：

```ts
"ABORT: unexpected string field null : 'YOUR DATA'".
```

你可以通过在前端调用你的智能合约，来解决这个问题。这是因为 NEAR 使用了一个 JSON-RPC-API, 所有的方法都是通过 _objects对象_ 来调用的。

所以不是这样调用：

```javascript
contract.someMethod("BAD"); // 错误的调用!
```

而是传入后端需要使用的 **object（对象）** 以及相应的变量名称，就像你在调用一个 REST API 一样。

```javascript
// 正确的调用!
contract.someMethod({
    myData: "GOOD"
})
```

### 2. 我尝试调用的函数在哪里?

你需要做两件事，以在前端调用你的智能合约函数。

1. 定义好你要在合约中调用的函数，并确认它们是公开的。 \(这一点你应该做得很好吧!\)
2. 在前端初始化合约时，声明你要调用的方法。 \(这一点你可能会遗漏，要注意哦！\)

```javascript
// 通过合约名称和合约配置，初始化合约API
window.contract = await near.loadContract(config.contractName, {
...
  // 查看的方法是只读的。它们不会更改状态，但通常会返回一些值。
  viewMethods: ["hello"],
  // 更改的方法可以更改状态。但是你不会收到返回值。
  changeMethods: [],
...
});
```

调用 `loadContract` 实际上是在声明一个带有`window.contract` 变量的对象，所以之后，你可以调用`window.contract.myFunction`。注意到 `window` 总是在内部的，所以你也可以直接调用 `contract.myFunction`。

### 3. 如何将数据存储到区块链中?

请查看我们的 [Data Storage 数据存储 / Collections 集合 ](/docs/concepts/data-storage) 文档，以深入了解如何在链上存储数据。

上面的链接详细说明了如何使用我们的这两种软件开发工具 (SDKs) 存储数据：

* [`near-sdk-rs`](https://github.com/near/near-sdk-as) for [Rust](https://www.rust-lang.org/)
* [`near-sdk-as`](https://github.com/near/near-sdk-as) for [AssemblyScript](https://www.assemblyscript.org/)

> 遇到困难了？在 [Discord channel （讨论频道）](https://near.chat) 中找到我们吧，我们将提供进一步的帮助！
