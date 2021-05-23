---
id: networks
title: NEAR 网络
sidebar_label: 网络
---

NEAR协议在若干个网络上运行，每个网络都基于自己独立的验证者和唯一的状态而运行。这些网络如下:

* [`mainnet (主网)`](/docs/concepts/networks#mainnet)
* [`testnet (测试网)`](/docs/concepts/networks#testnet)
* [`betanet (Beta测试网)`](/docs/concepts/networks#betanet)
* [`localnet (本地网)`](/docs/concepts/networks#localnet)
  
---

## `mainnet` (主网)

> `mainnet` 主要用于 投入实际生产的智能合约 和 实时的真实的代币传输。将用于`mainnet`的合约必须经过严格的测试，如有必要也应进行独立的安全性检查。`mainnet`是唯一一个状态(state)可以一直保留的网络 _（与通用的网络验证过程中的安全保障规范保持一致）_。


* [ [Status (状态)](https://rpc.mainnet.near.org/status) ]
* [ [Explorer (浏览器)](https://explorer.near.org) ]
* [ [Wallet (钱包)](https://wallet.near.org) ]

**请注意:** 在`near-cli`命令行工具中，用于 [选择主网](/docs/tools/near-cli#network-selection) 的 参数(flag) 是`production`。

---

## `testnet` (测试网)

> `testnet` 主要用于在部署`mainnet` 前，测试NEAR平台的方方面面。从帐户创建、模拟代币转移、工具开发到智能合约开发，`testnet` 环境都应尽量靠近`mainnet` 行为。我们将尽一切努力在迭代更新中维护这整个网络，但由于这个网络上运行着大量的测试所以并不稳定。因此，我们建议将所有面向用户的应用程序都部署到`mainnet`上以保证恒定的状态。

* [ [Status (状态)](https://rpc.testnet.near.org/status) ]
* [ [Explorer (浏览器)](https://explorer.testnet.near.org) ]
* [ [Wallet (钱包)](https://wallet.testnet.near.org) ]

**请注意:** 在`near-cli`命令行工具中，用于 [选择测试网](/docs/tools/near-cli#network-selection) 的 参数(flag) 为`development`_或_ `testnet`。 _(`near-cli`默认选择了测试网，所以可能不需要额外配置)_

---

## `betanet` (Beta测试网)

> `betanet` 通常每天都会发布尚未稳定的协议功能。我们会尽可能地维护状态(state)，但无法保证它的稳定性。

* [ [Status (状态)](https://rpc.betanet.near.org/status) ]
* [ [Explorer (浏览器)](https://explorer.betanet.near.org) ]
* [ [Wallet (钱包)](https://wallet.betanet.near.org) ]

`near-cli`命令行工具中，用于 [选择Beta测试网](/docs/tools/near-cli#network-selection) 的参数(variable) 是 `betanet`

---

## `localnet` (本地网)

> `localnet` 是为想要独立于公共区块链在 NEAR 平台上工作的开发者准备的。你需要自己生成节点。

更多有关本地开发的资料在 [这里](/docs/develop/node/running-a-node)

`near-cli`命令行工具中，用于 [选择本地网](/docs/tools/near-cli#network-selection) 的参数(variable) 是 `local`

---

>遇到问题了吗?
<a href="https://stackoverflow.com/questions/tagged/nearprotocol">
  <h8>在 StackOverflow 上提问吧!</h8>
</a>
