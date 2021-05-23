---
id: create-account
title: Creating a NEAR Account
sidebar_label: Create Account
---

创建NEAR账号最简单快捷的方式就是使用 [NEAR钱包 ](https://wallet.near.org/)。NEAR有若干基于不同的账号ID独立运作的 [开发网络](/docs/concepts/networks) 。下面，我们将给出在以下两种网络上创建账号的指南：

* [`测试网 testnet`](/docs/develop/basics/create-account#creating-a-testnet-account)
* [`主网 mainnet`](/docs/develop/basics/create-account#creating-a-mainnet-account)  

---

## 创建一个 `测试网` 账号

以下步骤将引导你使用 [NEAR钱包 ](https://wallet.testnet.near.org/)创建 `测试网testnet` 账号：

### 创建账号 ID

> * 进入 https://wallet.testnet.near.org ，点击 "Create Account"(创建账号)。

![mainnet wallet landing](/docs/assets/create-account/mainnet-wallet-landing.jpg)

> * 接着，输入你想要的账号名称。
  
![mainnet create account](/docs/assets/create-account/testnet-create-account.jpg)

---

### 完成账号安全设置

> * 选择你的账号恢复方法。我们推荐使用"Recovery Phrase"(助记词) 或 [Ledger](https://www.ledger.com/) ，是最安全的方法。

#### 用助记词恢复账号

> * 当选择使用"Recovery Phrase" / [seed phrase ](https://en.bitcoin.it/wiki/Seed_phrase)，请将你的助记词 **按正确的顺序** 记录下来并保存在一个安全的地方，这一点 **极为重要** ! 因为我们不会保留任何助记词的记录, 而没有助记词就不能恢复你的账号。

![recovery method selection](/docs/assets/create-account/security-method.jpg)

![setup seed phrase](/docs/assets/create-account/seed-phrase.jpg)

#### 使用电子邮箱 / 手机号恢复账号

> * 如果选择使用电子邮箱或手机号恢复账号, 在恢复账号时，我们就会发送一条 **一次性的** 包含了助记词的恢复链接到你的邮箱或手机。
>
> * **千万不要删除这条信息!** 一旦删除，我们是无法重新发送这条恢复链接的。另外，如果你丢失了你的邮箱账号或手机号，除非启用了另一种恢复方法,否则将导致帐户丢失.
![e-mail recovery](/docs/assets/create-account/email-text-recovery.jpg)

---

### 账号创建成功!

> 你刚刚成功创建了一个 `测试网` 账号并收到了 200 Ⓝ! 在确认账号恢复方法时，你应该会看到如下的账号管理页面:

![testnet success](/docs/assets/create-account/testnet-success.jpg)

> * 在这里，你可以查看你的总余额，可用余额，链上存储(on-chain storage costs) 所需的最小余额。你也可以查看你的 [访问密钥](/docs/concepts/account#access-keys) ，或通过激活 _(add)_ or 禁用 _(delete)_ 更改你的 [访问密钥](/docs/concepts/account#access-keys)。

---

## 创建一个 `主网` 账号

在 主网`mainnet` 上创建账号的步骤和测试网`testnet` 基本一致，但主网 `mainnet` 需要对账号进行初始充值。以下是 `主网` 账号创建的指南：

### 生成账号 ID

> * 打开 https://wallet.near.org ，点击 "Create Account"(创建账号)。

![mainnet wallet landing](/docs/assets/create-account/mainnet-wallet-landing.jpg)

> * 接着，输入你想要的账号名称。
  
![mainnet create account](/docs/assets/create-account/mainnet-create-account.jpg)

---

### 完成账号安全设置

> * 选择你的账号恢复方法. 我们推荐使用"Recovery Phrase"(助记词) or [Ledger](https://www.ledger.com/) ，是比较安全的方法。

#### 用Seed Phrase（助记词）恢复账号

> * 当选择使用"Recovery Phrase" / [seed phrase](https://en.bitcoin.it/wiki/Seed_phrase) ，请将你的助记词 **按正确的顺序** 记录下来并保存在一个安全的地方，这一点 **极为重要** ! 我们不会保留任何助记词的记录, 而没有助记词就不能恢复你的账号。

![recovery method selection](/docs/assets/create-account/security-method.jpg)

![setup seed phrase](/docs/assets/create-account/seed-phrase.jpg)

#### 使用电子邮箱 / 手机号恢复账号

> * 如果选择使用电子邮箱或手机号恢复账号, 在恢复账号时，我们就会发送一条 **一次性的** 包含了助记词的恢复链接到你的邮箱或手机。
>
> * **千万不要删除这条信息!** 一旦删除，我们是无法重新发送这条恢复链接的。另外，如果你丢失了你的邮箱账号或手机号，除非启用了另一种恢复方法,否则将导致帐户丢失.

![e-mail recovery](/docs/assets/create-account/email-text-recovery.jpg)

---

### 为你的账号充值

> * 创建账号及支付初始的存储费用需要 1.1 Ⓝ 的初始充值。你将会收到如下图所示的临时充值地址：

![fund your account](/docs/assets/create-account/fund-your-account.jpg)

> * 复制这个临时充值地址，并 **打开一个新的窗口** 进行充值。在进行账号初始充值时，切记将当前的页面保留在打开的状态。如果当前页面不小心关闭了，你可以按下面的格式重构页面链接: **wallet.near.org/fund-create-account/你的账号名称.near/临时充值地址**

![image](/docs/assets/create-account/url-breakdown.png)

> * 为了给新建账号充值，你可以选择使用已有的NEAR账号发送1.1 Ⓝ 或以上到这个临时充值地址；或选择点击 "Where can I purchase NEAR"(在哪里我可以购买NEAR) 转到交易所并购买一些NEAR。在购买时，你将会需要提供刚才的临时充值地址。

![purchase near](/docs/assets/create-account/purchase_near.jpg)

> * 当你的账号充值成功后，记得回到之前的"Fund Your Account"(为你的账号充值)窗口。这个页面会自动更新并提示你的账号已充值成功。要完成整个账号创建过程，还需要选中如下图所示的小方框，确认知道临时充值地址将被删除，之后发送到这个地址的任何资金都会丢失。

![image](/docs/assets/create-account/account-funded.png)

---

### 账号创建成功!

> * 你刚刚成功创建了一个 `主网mainnet` 上的NEAR账号!

![image](/docs/assets/create-account/mainnet-success.jpg)

> * 你应该会看到如下的账号管理页面。在这里，你可以查看你的总余额，可用余额，链上存储（on-chain storage costs）所需的最小余额。你也可以查看你的 [访问密钥](/docs/concepts/account#access-keys) ，或通过 激活 _(add)_ or 禁用 _(delete)_ 更改你的 [访问密钥](/docs/concepts/account#access-keys)。

![image](/docs/assets/create-account/mainnet-wallet-dashboard.jpg)

## 密钥存储与账号退出

> **请注意!** 在 _**账号退出前**_ 请确认你已经设置了至少一种有效的账号找回方法! 如果你没有设置账号找回方法，你 **将不能** 找回你的账号!
>
> 你或许已经发现，NEAR钱包里并没有一个"退出" 选项. 这是由于你的 [访问密钥](/docs/concepts/account#access-keys) 现在是存储在你的本地浏览器的。如果你必须要禁用浏览器本地存储的密钥，可以打开浏览器的开发者工具，清除相应的账号和密钥。

![local storage access key](/docs/assets/create-account/local-storage.png)

> 又或者，如果你想把 [访问密钥](/docs/concepts/account#access-keys) 存储在你的硬盘上，你可以使用 [`near-cli`](/docs/tools/near-cli) 命令行工具中的 [`near login`](/docs/tools/near-cli#near-login)命令。

## 帮助与支持
> 在创建账号过程中，遇到困难了吗？请在我们的 [#wallet-support](https://discord.gg/mGRcBpA8gN) 钱包支持频道中寻求相关的帮助与支持。
