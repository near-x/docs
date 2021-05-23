---
id: epoch
title: 纪元
sidebar_label: 纪元
---

> 一个 **纪元（epoch）** 是网络中的验证者保持不变时的时间单位。
> 
> - 测试网（`testnet`）和主网（`mainnet`）的纪元时长都是大约12小时，或者确切的说是43,200秒。
> - 您可以通过查询**[`genesis_config`](/docs/develop/front-end/rpc#genesis-config)** RPC端点和搜索`epoch_length`来查看这个(纪元时长的)设置。
> - 节点在5个纪元之后进行块的垃圾收集，除非它们是 [归档节点](/docs/roles/integrator/exchange-integration#running-an-archival-node).

**HTTPie 查询:**

```text
http post https://rpc.mainnet.near.org jsonrpc=2.0 id=dontcare method=EXPERIMENTAL_genesis_config
```

**Resonse 示例:**

```json
{
    "id": "dontcare",  //  <---------- ID号
    "jsonrpc": "2.0",  //  <---------- Jsonrpc版本号
    "result": {  //  <---------- 返回结果
        "avg_hidden_validator_seats_per_shard": [    //  <----------每个分片平均隐藏验证者席位 
            0
        ],
        "block_producer_kickout_threshold": 90,    //  <----------区块生产者剔除阈值
        "chain_id": "mainnet",  //  <----------区块生产者剔除阈值
        "chunk_producer_kickout_threshold": 90, //  <----------分片块生产者淘汰阈值
        "dynamic_resharding": false,  //  <----------动态分片
        "epoch_length": 43200, //  <---------- 以“秒”为单位的纪元时长
        "fishermen_threshold": "340282366920938463463374607431768211455",    //  <----------渔夫阈值
        "gas_limit": 1000000000000000,    //  <----------燃气限制
        "gas_price_adjustment_rate": [     //  <----------燃气价格调整率
            1,
            100
        ],
        "genesis_height": 9820210,      //  <----------距离创世区块高度
        "genesis_time": "2020-07-21T16:55:51.591948Z",    //  <----------创世区块时间
        "max_gas_price": "10000000000000000000000",     //  <----------最大燃气价格
        "max_inflation_rate": [     //  <----------最大通胀率
            0,
            1
        ],
        // ---- 节选 ----
}
```

您可以在[验证者常见问题解答](/docs/validator/staking-faq#what-is-an-epoch)中，了解更多关于纪元是如何被用来管理网络验证的信息。
> 遇到问题了吗?
  <a href="https://stackoverflow.com/questions/tagged/nearprotocol">
  <h8>在 StackOverflow 上提问吧！</h8></a>
