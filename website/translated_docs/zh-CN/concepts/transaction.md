---
id: transaction
title: 交易
sidebar_label: 交易
---


## 概述

一个交易是可以分配给网络的最小工作单位。 在这种情况下，工作是指计算（即执行一个函数）或存储（即读/写数据）。


## 交易过程

### 交易

一个`交易`是一个 `动作` 的集合，说明到达目的地（`接收者` 账户）应该做什么。

每项 `交易`都有关于以下方面的重要信息：
- **来源**（即由 `签名者`加密签字）。
- **目的地**或意图(即发送或适用于`接收者` )
- **最近一次交易** (即最近区块中的`block_hash`在可接受的范围内...) [1 纪元](/docs/concepts/epoch))
- **唯一性**(即 `随机数 `对于给定的`签名者` `访问密钥`必须是唯一的)

### 动作

一个  `动作`是一个可组合的操作单位，与零个或多个其他 `动作`一起定义一个合理的 "交易"。 目前有8种支持的`动作`类型。

- `创建账户(CreateAccount)`建立一个新的账户(为个人、合约、冰箱等)。
- `删除账户(DeleteAccount)`删除一个账户(并将余额转入受益人账户)
- `增加密钥(AddKey) `为账户添加一个密钥(`完全访问`或`调用合约`访问)
- `删除密钥(DeleteKey)`删除账户中的现有密钥。
- `转账(Transfer)`将代币从一个账户转移到另一个账户。
- `抵押(Stake)`表示有兴趣在下一次机会时成为验证者；
- `部署合约(DeployContract)`部署一个合约。
- `调用合约(FunctionCall)`调用合约上的一个函数(包括计算和存储的预算)


### 收据

`收据`是系统中唯一可操作的对象。当我们在NEAR平台上谈论 "处理交易 "时，最终意味着在某个时刻"申请收据"。

一个很好的心理模型是把`收据`看作是一个要在目的地（`接收者`）执行的付费消息。而`交易`是外部发出的创建`接收者`的请求（存在1对1的关系）。

创建`收据 `有几种方式。

- 发出`交易`
- 回报承诺(与交叉合约调用有关)
- 退款

你可以在[NEAR nomicon](https://nomicon.io/RuntimeSpec/Receipts.html)中找到更多关于 `收据`的技术细节。

### 原子性

由于交易在应用之前就已经转换为收据，所以谈一下收据的原子性就可以了。收据的执行是原子性的，也就是说，要么所有的操作都成功执行，要么都不成功。但是，有一个注意事项是，一个函数调用交易与其他交易不同，可以产生无限量的收据，虽然每张收据都是原子性的，但一张收据的成功或失败并不一定会影响其状态。
同一交易产生的其他收据的数量。

### 交易状态

可以通过[rpc](/docs/develop/front-end/rpc)查询交易的状态。查询结果的例子如下
```javascript
{
  status: { SuccessValue: '' },
  transaction: {
    signer_id: 'near',
    public_key: 'ed25519:5zset1JX4qp4PcR3N9KDSY6ATdgkrbBW5wFBGWC4ZjnU',
    nonce: 51,
    receiver_id: 'transfer-vote.near',
    actions: [
      { Transfer: { deposit: '50000000000000000000000000' } },
      [length]: 1
    ],
    signature: 'ed25519:37rcwcjDBWWAaaRYCazHY72sfDbmudYvtmEBHMFmhYEfWD3mbrgrtYs5nVh9gzRUESELRDET9g72LnAD2BWdSgKu',
    hash: 'EL9cEcoiF1ThH1HXrdE5LBuJKzSe6dRr7tia61fohPrP'
  },
  transaction_outcome: {
    proof: [ [length]: 0 ],
    block_hash: 'dvwSabiWzRjfQamZCEMeguxxXL4885JGU87xfjoPWR2',
    id: 'EL9cEcoiF1ThH1HXrdE5LBuJKzSe6dRr7tia61fohPrP',
    outcome: {
      logs: [ [length]: 0 ],
      receipt_ids: [ '6LrHPazG3DTcKkd4TjqbgajqmbcAfyoTG383Cft5SZ5Y', [length]: 1 ],
      gas_burnt: 223182562500,
      tokens_burnt: '22318256250000000000',
      executor_id: 'near',
      status: {
        SuccessReceiptId: '6LrHPazG3DTcKkd4TjqbgajqmbcAfyoTG383Cft5SZ5Y'
      }
    }
  },
  receipts_outcome: [
    {
      proof: [ [length]: 0 ],
      block_hash: '6evPKFQRw1E3gH9L1d59mz7GahsbnqsdYwcZQo8hpFQB',
      id: '6LrHPazG3DTcKkd4TjqbgajqmbcAfyoTG383Cft5SZ5Y',
      outcome: {
        logs: [ [length]: 0 ],
        receipt_ids: [ '7NMpF9ZGwSj48bpvJK2xVobJkTasEkakazTKi2zotHR4', [length]: 1 ],
        gas_burnt: 223182562500,
        tokens_burnt: '22318256250000000000',
        executor_id: 'transfer-vote.near',
        status: { SuccessValue: '' }
      }
    },
    {
      proof: [ [length]: 0 ],
      block_hash: 'Gm6TFS1ZxmA45itVj8a7vE8yJF8V5hXeNF1EhEVr7GVS',
      id: '7NMpF9ZGwSj48bpvJK2xVobJkTasEkakazTKi2zotHR4',
      outcome: {
        logs: [ [length]: 0 ],
        receipt_ids: [ [length]: 0 ],
        gas_burnt: 0,
        tokens_burnt: '0',
        executor_id: 'near',
        status: { SuccessValue: '' }
      }
    },
    [length]: 2
  ]
}
```
其中显示交易的整体状态、交易的结果和收据的结果。
`status`字段是一个单一键的对象。

`status`字段是一个对象，有一个键，可以有四种类型的键：
- `SucccessValue`。 这表示收据已经成功执行，键的值是返回值(只有当它是函数调用接收的结果时，才能为非空)。
- `SuccessReceiptId`。这表示一个交易已经成功转换为收据，或者收据是成功处理并生成另一张收据。这个键的值是新生成的收据的id。
- `Failure`这表示交易或收据在执行过程中失败。
- `Unknown`这表示交易或收据还没有被处理。

注意：对于收据，`SuccessValue`和`SuccessReceiptId`来自于最后一个动作的执行。同一收据中的动作执行不被退回。但是，如果任何动作执行失败，该收据的执行将被退回。停止，返回失败，也就是说`status`会是`Failure`。而如果最后一个动作不是函数调用，并且成功执行，那么结果将是一个空的`SuccessValue`。

最高级别的 "status "表示交易的总体状态。这表明交易中的所有行动是否都已完成，已被成功执行。但是，一个警告是，函数调用的成功执行并不一定意味着从函数调用中产生的收据都已成功执行。
例如： 

```rust
pub fn transfer(receiver_id: String) {
    Promise::new(receiver_id).transfer(10);
}
```
这个函数调用了一个承诺，但它的返回值与它所调用的承诺无关。所以即使承诺失败，有可能是因为`receiver_id`不存在，调用这个函数的事务仍然会有整体`status`中的`SuccessValue`。我们可以查看每张收据产生的状态，通过查看`receipt_outcomes`在同一查询结果中。

### 最终

交易的确定性与包含交易的区块的确定性紧密相关。但是，它们不一定相同，因为通常情况下，人们关注的是收据而非交易本身是否是已经确定的，因为收据执行是大部分工作要做的。为了确保交易的持久性，可以简单地查询交易并检查交易的所有区块哈希值以及从交易中产生的收据是否都是确定的。

>Got a question?
><a href="https://stackoverflow.com/questions/tagged/nearprotocol">
><h8>Ask it on StackOverflow!</h8></a>