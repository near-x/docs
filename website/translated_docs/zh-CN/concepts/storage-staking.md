---
id: storage-staking
title: 存储质押
sidebar_label: 存储质押
---

> 当你在NEAR上部署一个智能合约时，你会使用一种叫做存储质押(storage staking) 的机制来支付这个合约所需要的存储。
>
> 在存储质押（有时也被称为 _状态_ 质押, _state_ staking）中，拥有智能合约的账户必须根据该智能合约中存储的数据量对代币进行质押（或锁定），从而有效减少合约账户的余额。

<blockquote class="info">
<strong>来自以太坊？</strong><br><br>

如果你熟悉以太坊的定价模式，你可能会知道，与NEAR一样，该协议对每笔交易收取费用（称为 "gas"）。与NEAR不同的是，以太坊的 gas费 由该笔交易存储的数据量计算得出。这在根本上意味着任何人都可以支付一次费用来存储链上的永久数据。这是一个糟糕的经济设计，至少有两个原因：1. 运行网络的人（在以太坊1中cheng'we矿工）没有足够的激励来存储大量数据，因为在很久以前收取的gas费仍会一直增加存储成本；2. 智能合约的用户要为他们发送的数据存储在合约中而付费，而不是由智能合约的所有者付费。
</blockquote>

## NEAR的设计如何调整激励机制？

存储zhi型代币(Storage-staked token) 无法用于其他用途，比如验证质押。这增加了验证者将获得的收益率。更多信息请参见[经济学白皮书]（https://near.org/papers/economics-in-sharded-blockchain/）。

## 代币什么时候会被质押？

在每个添加数据的即将进行的交易中。

让我们通过一个例子来了解一下。

1. 你启动[一个留言簿应用](https://examples.near.org/guest-book)，将你的应用的智能合约部署到账户`example.near`中。
2. 你的应用程序的访问者可以将消息添加到留言簿。这意味着你的用户将 [默认](/docs/concepts/gas#what-about-prepaid-gas) 支付少量的gas费用，将他们的信息发送到您的合同。
3. 当这样的呼叫进来时，NEAR会检查`example.near`是否有足够大的余额，可以质押一笔钱来满足新的存储需求。如果没有，交易将失败。

## "百万廉价数据添加"攻击

请注意，这可能会产生一个攻击面。继续上面的例子，如果向你的留言簿发送数据的成本接近于零，而合约所有者的成本却大得多，那么恶意用户就可以利用这种不平衡，使维护合约的成本高得令人望而却步。

那么，在设计你的智能合约时要小心，确保这种攻击让潜在的攻击者付出的代价大于他做这件事情的价值。

## 另外，你可以删除数据来解押(unstake)一些代币

熟悉关于区块链的 "不可更改的数据" 表述的人会对这一点很惊讶。虽然一个 _索引节点_ 确实会永远保留所有数据，但 _验证节点_（即网络中大多数验证者运行的节点）却不会。智能合约可以提供删除数据的方法，这些数据将在几个[纪元](/docs/concepts/epoch)内从网络中的大多数节点中清除。

请注意，调用你的智能合约来删除数据会有相关的gas费。考虑到NEAR的gas费用限制，这就为单次交易中可以删除多少数据设置了一个上限。

## 它的成本是多少？

存储质押的价格是由网络设定的金额，初始化为 **[每字节1E20 yoctoNEAR](https://github.com/near/nearcore/blob/2141bdafc57def7793708dcfcbf6aaea4c56e2c5/neard/res/mainnet_genesis.json#L32)** ， 或**每个NEAR代币10kb（Ⓝ）**。

这个值在未来可能会改变。NEAR 的 JSON RPC API 提供了[查询这个初始设置的方法](/docs/develop/front-end/rpc#genesis-config)，但目前还没有提供查询 "实时" 配置值的方法。如果有任何变化，本文档将提前更新，以包含如何查询实时版本的信息。

## 成本明细示例

让我们通过一个例子来了解一下。

一个[不可互换的代币(NFT)](https://github.com/nearprotocol/NEPs/pull/4)是独一无二的，这意味着每个代币都有自己的ID。合约必须存储一个从代币ID到所有者账户ID的映射。

如果用这样的NFT来追踪**100万**个代币，那么代币ID到所有者的映射需要多少存储量？而这些存储需要多少代币作质押？

以[这个基本的AssemblyScript实现](https://github.com/near-examples/NFT/tree/master/contracts/assemblyscript/nep4-basic)为启发，让我们来计算一下使用`near-sdk-as`的[`PersistentMap` (永久映射)](https://near.github.io/near-sdk-as/classes/_sdk_core_assembly_collections_persistentmap_.persistentmap.html)时的存储需求。虽然它的具体实现可能会在未来发生变化，但在此时，`near-sdk-as`将数据存储为UTF-8字符串。下面我们就以此为假设的前提。

这就是我们的`PersistentMap`。

```ts
type AccountId = string
type TokenId = u64
const tokenToOwner = new PersistentMap<TokenId, AccountId>('t2o')
```

在幕后，NEAR区块链上存储的所有数据都保存在一个键-值数据库中。那个传递给`PersistentMap`的`'t2o'`变量帮助它跟踪记录它所有的值。如果你的账户`example.near`拥有ID`0`的token，那么在写这篇文章的时候，以下数据将被保存到键-值数据库中：

* 键: `t2o::0` 
* 值: `example.near`

那么对于100万的代币，下面就是我们需要加起来再乘以100万的东西。

1. 前缀 `t2o`将被序列化为UTF-8的三个字节，两个冒号将再增加两个。也就是5个字节。
2. 对于`TokenId`自动递增的实现，其值将在`0`和`999999`之间，这样平均长度为5个字节。
3. 我们假设这些NEAR`AccountId`结构良好，并且NEAR账户ID遵循与域名类似的模式——[平均约10个字符](https://www.domainregistration.com.au/news/2013/1301-domain-length.php)，再加上`.near`这样的顶级名称。所以一个合理的平均预期大约是15个字符；让我们做一个保守的估计，25个字符。这将等于25个字节，因为NEAR账户ID必须使用ASCII集的字符。

所以，我们的估计是：

    1_000_000 * (5 + 5 + 25)

3500万字节。乘以每字节1e20 yoctoNEAR，我们发现`tokenToOwner`映射需要质押3.5e27 yoctoNEAR或Ⓝ3,500。

请注意，你可以把前缀从`t2o`改成一个字符，从而把这个费用降为Ⓝ3,300。或者完全去掉它! 你可以在智能合约中的一个`PersistentVector(永久向量)`上有一个零长度的前缀；如果你这样做，你可以把它降为Ⓝ3.2。


## 为你自己的合约计算成本

就像上面所说的，手动进行字节计算是很困难的，而且容易出错。好消息是：你不必这样做!

你可以测试在单元测试中所使用的存储。

* 使用[`near-sdk-as`](https://near.github.io/near-sdk-as)，导入`env`并检查`env.storage_usage()` - [例子](https://github.com/near/near-sdk-as/blob/b308aa48e0bc8336b458f05a231409be4dee6c69/sdk/assembly/__tests__/runtime.spec.ts#L156-L200)

你也可以在模拟测试中测试存储；请查看[这个模拟测试示例](https://github.com/near-examples/simulation-testing)来开始。


## 其他降低成本的方法

对于运行网络的人来说，链上存储数据并不便宜，NEAR将这个成本转移到了开发者这边。那么，作为开发者，如何降低成本呢？有两种流行的方法：

1. 使用二进制序列化格式，而不是JSON；
2. 将数据存储在链外。

### 使用二进制序列化格式，而不是JSON。

NEAR核心团队维护了一个名为[borsh](https://borsh.io/)的库。
当你使用`near-sdk-rs`时，会自动使用到它。有一天，它可能也会被`near-sdk-as`使用。

想象一下，你想存储一个数组，比如"[0, 1, 2, 3]"。你可以将它序列化为一个字符串，并将其存储为UTF-8字节。这就是今天`near-sdk-as`所做的。去掉空格，你最终使用了9个字节。

使用borsh，同样的数组被保存为8个字节：

    \u0004\u0000\u0000\u0000\u0000\u0001\u0002\u0003

乍一看，节省1个字节似乎并不重要。但我们仔细看看。

这里的前四个字节，"/u0004u0000/u0000/u0000"，告诉序列发生器这是一个长度为`4`的`u32`数组，使用小端字节序编码(little-endian encoding)。剩下的字节就是数组里包含的数字– `\u0000\u0001\u0002\u0003`。当你序列化更多的元素时，每一个元素会给数据结构增加一个字节。在JSON中，每个新元素都需要增加两个字节，来代表数字和另一个逗号。

总的来说，Borsh速度更快，使用的存储空间更少，成本更低。如果可以的话，请使用它。

### 将数据存储在链外

如果你存储的是用户生成的数据，这一点尤为重要!

我们以这个[留言簿](https://github.com/near-examples/guest-book)为例。按目前的实现，访客可以用NEAR登录应用并留言。他们的留言会被存储在链上。

想象一下，这个应用变得非常流行，访客开始留下出人意料的长留言。合约所有者可能很快就会耗尽用于存储的资金!

更好的策略可能是将数据存储在链外。如果你想保持应用的去中心化，一个流行的链外数据存储方案是[IPFS](https://ipfs.io/)。有了它，你可以用一个可预测的内容地址来表示任何一组数据，比如:

    QmYwAPJzv5CZsnA625s3Xf2nemtYgPpHdWEz79ojWnPbdG

这样的内容地址可以代表一个JSON结构或图像或任何其他类型的数据。这些数据会被物理存储在哪里？你可以使用[Filecoin](https://filecoin.io/)或运行你自己的IPFS服务器来pin你的应用程序的数据。

通过这种方法，你添加到合约中的每一条记录都将有可预测的大小。

## 总结

NEAR的结构在激励网络运营商的同时，也为合约开发者提供了灵活性和可预测性。管理存储是智能合约设计的一个重要方面，NEAR的库可以轻松计算出你的应用需要多少存储成本。

>遇到问题了吗？
<a href="https://stackoverflow.com/questions/tagged/nearprotocol">
  <h8>在 StackOverflow 上提问吧!</h8></a>