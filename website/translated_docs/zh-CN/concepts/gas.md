---
id: gas
title: 燃气
sidebar_label: 燃气
---
当你调用NEAR区块链来更新或更改数据时，区块链基础设施的运营方会产生一些成本。可能造成的结果是，某些计算机在某处处理你的请求，而运营这些计算机的[验证者](/docs/validator/staking-overview)则花费了大量的资金来保持这些计算机的运行。

与其他可编程区块链一样，NEAR通过收取 _交易费_ (也称为 _gas费_ )来补偿这些人。

如果你熟悉web2云服务供应商(Amazon Web Services，Google cloud等)，那么你会发现与它们相比，区块链有一个很大的区别：当用户调用应用程序时，他们会立即被收取费用，而不是开发者承担使用所有基础设施的成本。这创造了新的可能性，例如应用程序不会有因为开发者或公司的资金耗尽而最终下线的风险。然而，它也带来了一些可用性的障碍。为了解决这一问题，NEAR还为开发者提供了[为用户支付gas费](#what-about-prepaid-gas)的功能，为web2使用者创造更熟悉的体验。

对于gas，请记住两个概念:

* **Gas单位**: 在内部，交易费用不是直接用NEAR代币来计算的，而是通过这种作为中间产物的“gas单位”来计算的。使用gas单位的好处是它们具有确定性——同样的交易总是花费相同数量的gas单位。
* **Gas价格**: 将Gas单位乘以Gas价格，就可以得出应该向用户收取的费用。这个价格是在每个区块内，根据网络需求自动重新计算的（如果前一个区块使用了一半以上，那么gas价格就会上涨；反之，gas价格就会下降；在每个区块内，价格波动幅度不会超过1%），并且存在一个由网络设定的最低限价，目前为1亿[yocto][metric prefixes] NEAR。

  [metric prefixes]: https://www.nanotech-now.com/metric-prefix-table.htm

请注意，gas价格在NEAR的主网和测试网之间可能有所不同。 在以下数字之前，可以[看一下Gas价格](#whats-the-price-of-gas-right-now) 。

## 以Gas计算

NEAR有一个大约1秒的区块时间，通过[限制](https://github.com/near/nearcore/blob/49b4fcdc297a609d8bb38858fdbf71e7d821f1f5/neard/res/genesis_config.json#L189)每个区块内的Gas数量来实现。而Gas单位则是一个经过了精心计算而得到的易于记忆和使用的数字:

* 10¹² Gas单位, 或者 **1 TGas** (_[兆][metric prefixes]Gas_)...
* ≈ **1 毫秒** "计算(compute)" 时间
* ...若以最低汽油价格 1亿 yoctoNEAR 来计算, 等于 **0.1 milliNEAR** 的费用

这个“1毫秒”是一个粗略但有用的近似值，也是当前在NEAR中设定Gas单位的目的。Gas单位不仅包含了计算/CPU时间，也包含了带宽/网络时间和存储/IO时间。通过治理机制，在未来可以通过调整TGas和毫秒之间的对映关系、调整系统参数，但以上关于Gas单位的描述仍然是理解和思考Gas单位来源和意义的一个很好的起点。


## 常见操作的费用

为了给你初步了解NEAR上的费用，下表列出了一些常见的操作以及它们目前所需的TGas数量，及以milliNEAR为单位、以最低价1亿 yN所计算得到的Gas费。


| 操作           | TGas | 费用 (mN) | 费用 (Ⓝ)
| ------------------- | ---- | -------- | -------
| 创建账户      | 0.42 | 0.042    | 4.2⨉10⁻⁵
| 发送资金          | 0.45 | 0.045    | 4.5⨉10⁻⁵
| 质押               | 0.50 | 0.050    | 5.0⨉10⁻⁵
| 添加完全访问密钥 | 0.42 | 0.042    | 4.2⨉10⁻⁵
| 删除密钥          | 0.41 | 0.041    | 4.1⨉10⁻⁵

<blockquote class="info">
<strong>进一步思考</strong><br><br>

这些数字是怎么来的？

NEAR[配置](https://github.com/near/nearcore/blob/49b4fcdc297a609d8bb38858fdbf71e7d821f1f5/neard/res/genesis_config.json#L57-L120)了基本费用。举一个例子:

    create_account_cost: {
      send_sir:     99607375000,
      send_not_sir: 99607375000,
      execution:    99607375000
    }

这里“sir”是“发送者即接收者”的缩写。是的，这些都一样，但在未来可能会改变。

当你请求创建一个新帐户，NEAR立即从你的帐户中扣除了相应的“发送”(`send`)金额。然后，它创建了一个 _receipt_ (收据)，这是一种内部的簿记机制，用于支持NEAR的异步分片设计（如果你是从以太坊过来的，请暂时忘记以太坊收据，因为它们完全不同）。创建一个收据具有自己的[相关费用](https://github.com/near/nearcore/blob/49b4fcdc297a609d8bb38858fdbf71e7d821f1f5/neard/res/genesis_config.json#L40-L44)：

    action_receipt_creation_config: {
      send_sir:     108059500000,
      send_not_sir: 108059500000,
      execution:    108059500000
    }

创建此收据所对应的“发送”(`send`)金额也会立即从你的帐户中扣除。

直到进入下一个区块，“创建帐户”操作才会结束。此时，每个操作的“执行”(`execution`)金额将从你的帐户中扣除（有些微妙的不同是：下一个区块的Gas单位将被乘以最多相差1％的Gas价格，因为Gas价格是在每个区块上重新计算的）。将所有这些加起来即可得出总的交易费用：


    (create_account_cost.send_sir  + action_receipt_creation_config.send_sir ) * gas_price_at_block_1 +
    (create_account_cost.execution + action_receipt_creation_config.execution) * gas_price_at_block_2
</blockquote>

## 复杂操作的费用

上面的数字应该会让你觉得在NEAR上进行交易非常便宜！但是，上面的数字并不能告诉你，使用一个更复杂的应用程序或运营一项基于NEAR的业务要花费多少。让我们来介绍一些更复杂的gas计算：部署合约和调用函数。

### 部署合约

对于部署合约来说，[基本操作成本](https://github.com/near/nearcore/blob/49b4fcdc297a609d8bb38858fdbf71e7d821f1f5/neard/res/genesis_config.json#L57-L120)包括两种不同的值。简化了一下,它们是:


    deploy_contract_cost: 184765750000,
    deploy_contract_cost_per_byte: 6812999,

第一项是基线成本，与合约大小无关。请记住，它们中的每一项都需要乘以2，分别对应“发送”`send`和“执行”`execution`成本，并且也需要发送和执行收据（参见上面的蓝色框），最终所需的Gas单位为：

    2 * 184765750000 +
    2 * contract_size_in_bytes * 6812999 +
    2 * 108059500000

(结果除以10¹²就可以得到TGas数量!)

请注意，这里面包括了上传和将字节写入到存储中的费用，但不包括在存储中保存这些字节的成本。长期存储可以通过[存储质押]来补偿，这是一个可恢复的根据字节计算费用的数额，这个数额在合约部署期间也会从你的账户中扣除。

在这个[可替代代币示例](https://github.com/near-examples/FT/pull/42)中，AssemblyScript合约编译后的大小刚好超过16kb（Rust合约要大得多，但这[即将优化](https://github.com/near/near-sdk-rs/issues/167)）。使用上面的计算方法，我们发现部署该合约需要 **0.81 TGas** 的交易费用（即0.081mN，以最低Gas价格计算），而 1.5N 将被锁定以进行存储质押。


### 函数调用

基于NEAR的多用、通用的本质，函数调用才是Gas计算中最最最复杂的！一个既定的函数调用将使用难以预测数量的CPU，网络，IO数量，而这每一个的数量甚至会随着合约中已存储数据的数量而变化！

在这种复杂程度下，通过一个示例来了解、[列举每一个](https://github.com/near/nearcore/blob/f816d09ad634071aff20ad1c71aaf0f6886742d5/neard/res/genesis_config.json#L135-L185) Gas的计算式就不再有用了。（[感兴趣的话](https://github.com/near/nearcore/blob/f816d09ad634071aff20ad1c71aaf0f6886742d5/neard/res/genesis_config.json#L135-L185)，也可以自己探索一下）。
让我们从两个不同的角度来切入这个问题：类似以太坊的估算方法，以及通过自动化测试获得精准的估计值。

#### 类似以太坊的估算方法

像NEAR一样，以太坊使用Gas单位来模拟某一项操作的计算复杂度。但与NEAR不同，以太坊用的不是可预测的Gas价格，而是使用动态的、以竞价拍卖为基础的市场。这使得我们模仿以太坊的Gas价格估算有一些棘手，但我们会尽力而为。

Etherscan给出了一个[以太坊Gas价格历史走势图](https://etherscan.io/chart/gasprice)。这些价格是以 “Gwei” 或 Gigawei 为单位的，其中 wei 是 ETH 的最小可能数量，即10⁻¹⁸。从2017年11月到2020年7月，平均Gas价格为 21Gwei。让我们称其为“平均”(average)Gas价格。在2020年7月，平均Gas价格上涨至57Gwei。让我们称其为“高位”(high)以太坊Gas费。

将以太坊的Gas单位乘以Gas价格通常会得出一个以milliETH (mE)表示的数量，这与我们将NEAR的TGas转换为milliNEAR的方式相似。让我们再看一看两者的一些共有操作，将ETH的Gas单位类比为NEAR的Gas单位，也将上述的“平均”(average)和“高位”(high)Gas价格进行类比和转换。

| 操作                                       | 以太坊Gas单元 | 平均mE | 高位mE | NEAR TGas            | 豪N
| ----------------------------------------------- | ------------- | ------ | ------- | ---------            | -----
| 传输本地代币 (ETH 或者 NEAR)             | 21k           |  0.441 |  1.197  |   0.45               | 0.04
| 部署并初始化 [可替代代币] 合约 | [1.1M]        | 23.3   | 63.1    |  [9]<super>†</super> | 0.9 (由于[存储质押]还需要加1.5Ⓝ)
| 传输可替代代币                      | [~45k]        |  0.945 |  2.565  | [14]                 | 1.4
| 为可替代代币设置暂交第三方保管的保证金          | [44k]         |  0.926 |  2.51   |  [8]                 | 0.8
| 查看可替代代币的余额         | 0             |  0     |  0      |   0                  | 0

<super>†</super>
另外，函数调用需要启动虚拟机、并将所有已编译的Wasm字节加载到内存中，因而比基本操作增加了费用；这个目前[正在优化中](https://github.com/near/nearcore/issues/3094)。

一些操作在表面上看起来比以太坊提升了大约10倍，但需要注意的是NEAR的总供给超过10亿，而以太坊的总供给则在1亿左右。因而作为总供应量的一部分，NEAR的Gas费降到了以太坊Gas的100分之一左右(approximately another 10x lower)。此外，如果NEAR的价格大幅上涨，则可由网络所设定的最低Gas费还能变得更低。

网络流量往往会归属于最低的gas价格(you can expect the network to sit at the minimum gas price most of the time)；在[经济白皮书](https://near.org/papers/economics-in-sharded-blockchain/#transaction-and-storage-fees)中可以了解更多信息。


  [fungible token]: https://github.com/near-examples/FT/pull/42
  [1.1M]: https://github.com/chadoh/erc20-test
  [9]: https://explorer.testnet.near.org/transactions/GsgH2KoxLZoL8eoutM2NkHe5tBPnRfyhcDMZaBEsC7Sm
  [storage staking]: ./storage
  [~45k]: https://ethereum.stackexchange.com/a/72573/57498
  [14]: https://explorer.testnet.near.org/transactions/5joKRvsmpEXzhVShsPDdV8z5EG9bGMWeuM9e9apLJhLe
  [8]: https://explorer.testnet.near.org/transactions/34pW67zsotFsD1DY8GktNhZT9yP5KHHeWAmhKaYvvma6
  [44k]: https://github.com/chadoh/erc20-test


#### 通过自动化测试获得精准的估计值

我们将在近期演示如何进行深度的Gas费用估算；你可以 [订阅这个问题](https://github.com/near/devx/issues/253)以获取更新。在此之前，你可能需要看一下这个[如何进行模拟测试的示例](https://github.com/near-examples/simulation-testing)，这是一种测试合约并检查合约执行的方方面面的非常有效的方法。

如果你正在使用NEAR的AssemblyScript SDK，你可以使用[两种方法](https://github.com/near/near-sdk-as/blob/741956d8a9a44e4252f8441dcd0ba3c19743590a/assembly/runtime/contract.ts#L68-L81)， `context.prepaidGas`和`context.usedGas`。这些方法既可以与测试一起使用，也可以不与测试一起使用；它们可用于报告在你的合约方法执行时，虚拟机中关联gas数量及消耗量的信息：


```ts
/**
* 获得关联到这个调用上的gas单元数量
*/
get prepaidGas(): u64 {
 return env.prepaid_gas();
}

/**
* 获得在合约执行中已经消耗的gas单元数量
* 和关联到promise上的gas单元数量 (不能超过预付的gas数量)。
*/
get usedGas(): u64 {
 return env.used_gas();
}
```


## 我怎么购买gas？

你不能直接购买gas；你可以做的是将代币附加到交易中。

调用NEAR读取数据始终是免费的。但是，当你调用函数以添加或更新数据，您必须这样做：从一个有NEAR代币可用余额的账户中，附加代币到交易中，以支付gas费。

如果你来自以太坊，你可能习惯于支付更高的gas费以使你交易更快处理。但是在NEAR，gas费用是确定的，你不可以额外支付。

对于像“传输资金”这样的基本操作，您不能指定要附加的gas数量。所需的gas很容易提前计算，所以它会自动为你附加上。(请查看:[ near-cli ](https://github.com/near/near-cli)中有一个`send`命令，它不接受`gas`参数;[ near-api-js ](https://github.com/near/near-api-js)有一个[`sendTokens` ](https://near.github.io/near-api-js/classes/_near_.near.html#sendtokens)函数，它也不接受`gas` 自变量。)如上表所示，这些操作很便宜，所以你甚至可能注意不到帐户余额的轻微减少。

函数调用更为复杂，你可以对这些交易中附加明确数量的gas，上限为 3⨉10¹⁴ gas单位 这个[最大值](https://github.com/near/nearcore/blob/c162dc3ffc8ccb871324994e58bf50fe084b980d/neard/res/mainnet_genesis.json#L193)。如下，你可以在[`near-cli`](https://github.com/near/near-cli)中这样覆盖默认的附加gas数量：

    near call myContract.testnet myFunction "{ \"arg1\": \"val1\" }" --gas=300000000000000
并且在[`near-api-js`](https://github.com/near/near-api-js)中，您还可以在调用change方法时明确指定要附加的gas单位的数量；参见[此处的示例](https://github.com/near-examples/guest-book/blob/ceb2a39e53351b4ffc21d01987e2b0b21d633fa7/src/App.js#L29)。


而调用这个解决方案的警示错误则是像这样的:

_（错误：
A9BzFKmgNNUmEx9Ue9ARC2rbWeiMnq6LpcXh53xPhSN6 交易失败了。超过了预付的gas数量。）_

```text
Error:
  Transaction A9BzFKmgNNUmEx9Ue9ARC2rbWeiMnq6LpcXh53xPhSN6 failed.
  Exceeded the prepaid gas
```

<blockquote class="warning">
<strong>这些gas单位会耗费多少代币?</strong><br><br>


请注意，你正在授权的是最大数量为Gas _单位_，而不是NEAR代币或者yoctoNEAR的数量。

这些单位将乘以它们所在区块的gas价格。如果函数调用进行了跨合约的调用，那么函数的各个部分将在不同的区块中进行处理，从而使用不同的价格计算。如[上方蓝色框](#the-cost-of-common-actions)中所述，这个函数至少需要两个区块才能完成。

假设系统在整个操作过程中维持在1亿yoctoNEAR的最低gas价格上，那么最大附加gas数量将是3⨉10¹⁴ ，所以最大支出为3⨉10²² yN。另外，出于保守的估计我们还需要再乘以大约6.4的倍数，以[防止分片拥塞](https://github.com/nearprotocol/NEPs/issues/67)。

将这所有三个数字相乘，我们会发现，如果gas价格保持在最低水平，那么最大数量的附加gas单位将使操作花费约0.2Ⓝ。如果gas价格高于最低价，这项收费可能会更高。

如果在最开始的区块中，gas价格是最低的，但是相应的操作需要在多个区块完成，而后面的区块gas价格更高，会怎么样呢？费用会不会超过~0.2Ⓝ呢? 不会的，因为我们已经通过乘以6.4做了保守估计，涵盖了这种可能性。


## 附加多余的gas; 将会被退还!

怎么才能知道调用一个函数时需要附加的准确的gas数量呢？没法知道！

Gas单位数量是根据给定操作的计算复杂度计算得出的，而该复杂度可能会受到智能合约状态的影响。所以这很难提前预测。而且，gas价格是根据前一个区块中网络的繁忙程度，在每个区块进行调整的，这也很难提前预测。

但是好消息是！

* 在NEAR上，gas费用不高。
* 如果你附加的gas比需要的多，你就会得到退款

对于基本操作也是如此。在前一节中，我们提到了这些是自动计算和附加的。事实上,考虑到在这些操作执行时，gas价格可能都会有轻微波动(见[上面](#the-cost-of-common-actions)的蓝色框),所以在附加时都会轻微地进行额外附加,而任何超出所需的数量最终都将被退回。


## 预付的Gas怎么办??

Near团队明白，开发人员希望为用户提供最好的使用体验。为了实现这一愿景，开发人员可以设计他们的应用程序，让新用户可以直接从开发人员维护的账户中直接提取购买gas的资金。一旦用户开始使用应用，他们就可以为自己在平台上的使用情况付费。

从这个意义上说，为新用户预付gas可以通过一个资金账户和相关的合约来实现。


*那么，开发者要怎样为他们的NEAR用户支付gas费呢?*

在这个dApp上，用户可以直接从开发者帐户中使用仅适用于gas费的资金。然后，开发人员必须根据签名者的密钥(而非帐户名称)来区分用户。

请查看 [关键概念：帐户](https://github.com/near-x/docs/blob/zh/docs/concepts/account)， “你知道吗？”章节，第“＃2”项

NEAR协议对开发资金的使用并没有提供任何的限制功能。开发人员可以在与特定用户相对应的访问密钥上设置金额 -- 每个有特定金额的新用户一个`FunctionCall`访问密钥。


## 现在的gas价格是多少?


您可以使用RPC方法`gas_price`,直接在NEAR平台上查询特定区块上的gas价格。此价格可能会根据网络负载而变化。价格以 yoctoNEAR(10^-24 NEAR) 计价

1. 使用 [NEAR浏览器](https://explorer.testnet.near.org/blocks) 从区块链获取最近的区块哈希值(block hash)。

   *撰写本文时，最近的区块哈希值是                `SqNPYxdgspCT3dXK93uVvYZh18yPmekirUaXpoXshHv`*

2. 使用[文档里的](/docs/develop/front-end/rpc)`gas_price`方法，在这个区块发出一个关于gas价格的RPC请求

   ```bash
   http post https://rpc.testnet.near.org jsonrpc=2.0 method=gas_price params:='["SqNPYxdgspCT3dXK93uVvYZh18yPmekirUaXpoXshHv"]' id=dontcare
   ```

3. 观察结果

   ```json
   {
     "id": "dontcare",
     "jsonrpc": "2.0",
     "result": {
       "gas_price": "5000"
     }
   }
   ```

在这个区块，每单位gas的价格是 5000 yoctoNEAR (10^-24 NEAR)。


## 白皮书中的一些结论性观点

<blockquote class="info">
从根本上说，NEAR平台是自发的参与者之间的市场。在供应方，验证者节点和其他基础设施的运营人员需要激励，以提供构成“社区云”的这些服务。在需求方，开发人员和终端用户需要以一种简单、清晰、一致的方式在平台上使用并付费。

基于区块链的云(cloud)为运行在其上的应用程序提供了一些特殊的资源：


-**计算（CPU）**：这是运行合约代码的实际计算机处理量（以及立即可用的RAM）。

-**带宽（“网络”）**：这是参与者和用户之间的网络通信，包括提交交易的消息和传播区块的消息。

-**存储**：链上的永久数据存储，通常表示为一个包含存储空间和时间的函数。

现存的像以太坊这样的区块链，会在一次交易前费用中就对上述三种资源都进行计价(account for all of these in a single up front transaction fee which represents a separate accounting for each of them)，但最终只向开发人员或用户收取一次费用。这种被称为“gas”的费用波动非常大。

开发人员更喜欢可预测的价格机制，在这样的机制下他们才能做好预算并为终端用户提供报价。在NEAR上，上述资源的价格是根据系统使用情况缓慢调整的（服从于极端使用情况下重新分片的平滑效果），而不是完全基于竞价拍卖的。这意味着开发人员能更好地预测运行交易或维护其存储的成本。
</blockquote>

如果想要更深入地了解gas在NEAR上工作的方式和原因，请查看主白皮书的[经济](https://near.org/papers/the-official-near-white-paper/#economics)章节和经济学白皮书的[交易和存储费](https://near.org/papers/economics-in-sharded-blockchain/#transaction-and-storage-fees)章节。

>遇到问题了吗?
<a href="https://stackoverflow.com/questions/tagged/nearprotocol">
  <h8>在 StackOverflow 上提问吧!</h8></a>
