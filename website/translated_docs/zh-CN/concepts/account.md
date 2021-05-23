---
id: account
title: 账户
sidebar_label: 账户
---

NEAR使用人类可读的帐户ID，而不是公钥哈希(public key hash)。你可以查看这个YouTube上的这个 [“边吃边学”小视频(Lunch and Learn)](https://www.youtube.com/watch?time_continue=18&v=2_Ekz7w6Eo4&feature=emb_logo) ，里面有20分钟左右的视频解说。

## 账户ID规则

- 最小长度为2位字符
- 最大长度为64位字符
- `Account ID (账户ID)` 由 `Account ID parts (账户ID部件)` 组成，中间用`.`隔开
- `Account ID parts (账户ID部件）` 由小写字母和数字组成，用`_`或`-`隔开。

帐户名(Account name)类似于域名。任何人都可以创建没有分隔符的顶级帐户(top level account，TLA)，例如`near`。但只有`near`才能创建`alice.near`，只有`alice.near`才可以创建`app.alice.near`，以此类推。请注意，`near`是不能直接创建`app.alice.near`的。

一个完整的账户ID (如果不管长度限制的话)，它的正则表达式是这样的： `^(([a-z\d]+[\-_])*[a-z\d]+\.)*([a-z\d]+[\-_])*[a-z\d]+$`

---

## 顶级账号 (Top Level Accounts)

顶级帐户名(TLAs)非常有价值，因为它们为公司、应用程序和用户提供了信任和接触的基础。为了公平地使用它们，小于`MIN_ALLOWED_TOP_LEVEL_ACCOUNT_LENGTH (顶级账户名允许的最小字符长度)` (目前是32)的顶级帐户名将会被竞价出售。

具体来说，只有`REGISTRAR_ACCOUNT_ID (域名注册商账户ID)`可以创建小于`MIN_ALLOWED_TOP_LEVEL_ACCOUNT_LENGTH (顶级账户名允许的最小字符长度)`的新顶级帐户。`REGISTRAR_ACCOUNT_ID (域名注册商账户ID)`有一个标准的`Account Naming (帐户命名)`接口(interface)，可用于创建新的帐户。

我们不准备在一开始就实行`registrar (域名注册商)`拍卖。我们计划在未来的某个时间点，再由 Near 基金会实行拍卖。

目前，所有的`mainnet (主网)`账户都使用了`near`顶级账户名（如：`example.near`）, 所有`testnet (测试网)`帐户都使用了`testnet`顶级帐户（如：`example.testnet`)。

---

## 子账号 (Subaccounts)

如上所述，NEAR上的帐户名称与网站域名的命名模式相似，命名规则也相似。帐户可以创建任意数量的子帐户，且只有母帐户可以创建子帐户。例如：
`example.near`可以创建`subaccount1.example.near`和`subaccount2.example.near`，但**不能**创建`sub.subaccount.example.near`，因为只有 `subaccount.example.near` 可以创建`sub.subaccount.example. near`。同样的，`test.near`不能创建`subaccount.example.near`，只有它的直接母帐户有权限创建这个子帐户。

试着在终端使用我们的[`near-cli`](/docs/tools/near-cli)命令：[`near create-account`](/docs/tools/near-cli#near-create-account)，创建账号吧。

---

## 开发账户 (Dev Accounts)

开发帐户是通过 `near cli (NEAR命令行工具)` 和 `wallet (NEAR钱包)`等工具自动生成的一种特殊帐户，可以帮你将测试和部署合约的过程自动化。由于每个帐户都可以有一个合约，但重新部署合约**并不会**创建新的状态(state)，因此在测试合约时，你往往会想要将合约部署到不同的帐户中。

> **请注意：** 当部署多个测试示例并创建新的开发帐户时，你需要先在示例的`本地(localhost)` “登出” NEAR钱包，然后再 “登录” ！每一次登录会给你的帐户添加一个访问密钥，将私钥保存在本地，这样你的应用程序就可以直接调用合约而无需再次请求授权。**但是**！也有一种可能是你现在正尝试与一个部署在完全不同的开发帐户上的合约进行交互。

### 如何创建开发帐户？

- 当你在 NEAR命令行工具(near-cli) 中运行 `dev-deploy` 命令，它会去查找`/neardev/dev-account`底下的一个文件，这个文件包含了将要用来部署合约的开发账户ID。

- 如果找不到这样一个文件，它会创建一个开发帐户（使用我们的云助手服务创建测试帐户），然后为你创建这个包含了`开发帐户(dev-account)`文件的文件夹。

- 它还将在这个路径下创建相关的凭证(credentials)——一个公私密钥对(a public and private keypair)： ``~/.near-credentials/default/[dev-account-id].json``。试试这条命令:
```
code ~/.near-credentials/default/[dev-account-id].json
```
- 在这个路径` /neardev/dev-account`（需将"dev-account-id"替换为当前的开发账号id) 下，打开json文件，这里可以选择你自己偏好的编辑器(如VS Code)。

### 如何再创建一个开发账户？
- 删除文件夹` /neardev` 并运行` near dev-deploy[ wasmFile default="/out/main.wasm"]` ，你就会看到` neardev` 中创建了一个新的开发帐户，并且为你保存了相关的凭证(credentials)。
### 现在，我有一个开发账户了，然后呢？
- json文件中的这些帐户和相关密钥对对你自动化整个测试过程非常有帮助。
- NEAR生态系统中的许多例子都用到了 `yarn dev:deploy` 脚本，来部署合约、甚至运行一些测试。了解这些帐户是如何创建的、它们的凭证(credentials)存储在哪里、你可以如何使用它们，十分重要。

---

## 访问密钥 (Access Keys)

NEAR使用了可读性高的帐户ID，而非公钥哈希。而为每个账号所创建的多个密钥 ([公/私密钥对,public/private key pairs](https://en.wikipedia.org/wiki/Public-key_cryptography))，我们就称之为“访问密钥(Access Keys)”。目前，有两种类型的访问密钥： `FullAccess(完全访问)`和 `FunctionCall(函数调用)`。

### 完全访问密钥 (Full Access Keys)

顾名思义，`FullAccess(完全访问)` 对帐户具有完全的控制权，类似于对你的操作系统具有管理员权限。使用这个密钥，你可以在NEAR上完成以下8种操作，而不受任何限制：

1) 创建帐户
2) 删除账户
3) 添加密钥
4) 删除密钥
5) 部署合约
6) 调用函数
7) Ⓝ转账
8) Ⓝ质押（仅限于验证节点validators）

更多细节可以请查看 [操作说明(action specificitions)](https://nomicon.io/RuntimeSpec/Actions.html) 。

### 函数调用密钥 (Function Call Keys)

`FunctionCall(函数调用)` 密钥都是独一无二的，因为它只有权调用某一个或者某几个收取Ⓝ费用的智能合约方法 (即“需要付费的函数”，payable functions)。这种密钥具有以下三个属性：

1) `allowance (限额)` -加载到密钥上、用于支付合约调用费用的Ⓝ数额 _（默认为0.25)_
2) `receiver_id (接收者ID)` -密钥允许调用的合约的位置 _（必须项）_
3) `method_names (方法名)` -密钥允许调用的合约的方法名 _（可选项）_

> **请注意:** 如果没有指定特定的方法名，所有的方法都可以被调用。

在dApp中，创建 `函数调用` 密钥最简单的方法，就是通过 `NEAR-api-js` 的 [钱包连接](https://github.com/near/near-api-js/blob/0aefdb01a151f7361463f3ff65c77dbfeee83200/lib/wallet-account.js#L13-L139) 登录 [NEAR钱包](https://wallet.testnet.near.org/) 时的登录对话框。这个登录对话框向用户请求访问授权，一经授权，就会创建一个 `函数调用` 密钥。这个密钥，只允许调用将用户重定向到NEAR钱包的合约的方法，并附带有默认为0.25Ⓝ的金额，用于涵盖交易费用。当使用此密钥执行非货币性交易时，你会发现限额(allowance)在减少，一旦0.25Ⓝ被消耗完，你将需要创建一个新的密钥。如果使用 `函数调用` 密钥来请求转账**任意**数量的代币(token)，用户将被重定向到钱包以授权此交易。你可以跟着[NEAR 用户手册(Guestbook)](https://near-examples.github.io/guest-book/)试一试，以更好地了解和应用这项功能。

创建 `函数调用` 密钥的另一种方法是使用 `near cli (NEAR命令行工具)` 的 [`add-key`(添加密钥)](/docs/tools/near-cli#near-add-key) 命令。使用 `near-cli` ，你可以更精准地使用 `函数调用` 密钥，比如只允许它调用一个特定的合约方法，或者调整付费限额(allowance amount)。

 `函数调用` 访问密钥是一个非常强大的NEAR功能，它开启了许多可能性。用户不仅不再需要反复授权小额交易，甚至允许用户无需创建帐户即可与区块链进行交互（实现方式是：让dApp自动创建一个 `函数调用` 密钥，在前端页面点击操作使密钥指向它自己，就可以让任何人与你的dApp无缝交互）。

---
## 与以太坊的区别 (Compared to Ethereum)

如果你熟悉以太坊开发，那么可以快速了解一下二者帐户的区别。下图总结了一些关键的区别：

![Ethereum 账户 vs NEAR 账户](/docs/assets/accounts-compare-ethereum-v-near.png)

_图片来源：medium.com/@clinder_

---

## 账户与合约 (Accounts and Contracts)

每一个NEAR账户只能持有1个智能合约。对于应该需要组织多个合约的应用程序，您可以创建“主帐户（master account）”也就是用户帐户下的“子帐户(sub accounts)”。子帐户的名称包含 . 符号，就像`contract1.user-A-account`、`contract2.user-A-account` 等。NEAR不允许直接创建名称中带有 . 的帐户，这样，这些带 . 的帐户便只能由 `user-A-account` 创建，就仿佛用户帐户是一个顶级域名一样(就如同你所熟悉的模式中的 `your-company.com`)。

使用NEAR CLI (NEAR命令行工具)，你可以像这样将新合约部署到你的帐户上：

```bash
near deploy --wasm-file path/to/contract.wasm --account-id contractAccount.developerAccount.testnet --master-account yourAccount.testnet
```

请注意，要正确运行上述命令，你需要先登录 NEAR CLI 并授权它使用你的主帐户(master account,`YOUR_ACCOUNT.testnet`)。点击这里可查看更多关于 [NEAR CLI](/docs/tools/near-cli) 的信息。

> 遇到问题了吗？
> <a href="https://stackoverflow.com/questions/tagged/nearprotocol">在 StackOverflow 上提问吧！</a>

