---
id: epoch
title: 纪元
sidebar_label: 纪元
---

> 一个 **纪元** 是网络中的验证者恒定的时间单位.
> 
> - NEAR主网和测试网都有~12小时或者确切的说43,200秒的纪元时间间隔。  
> - 您可以通过查询**[`genesis_config`](/docs/develop/front-end/rpc#genesis-config)** RPC端点和搜索epoch_length来查看此设置值。
> - 节点垃圾在5个epoch之后收集块，除非它们是 [归档节点](/docs/roles/integrator/exchange-integration#running-an-archival-node).

**HTTPie Query:**

```text
http post https://rpc.mainnet.near.org jsonrpc=2.0 id=dontcare method=EXPERIMENTAL_genesis_config
```

**Example Resonse:**

```json
{
    "id": "dontcare",
    "jsonrpc": "2.0",
    "result": {
        "avg_hidden_validator_seats_per_shard": [
            0
        ],
        "block_producer_kickout_threshold": 90,
        "chain_id": "mainnet",
        "chunk_producer_kickout_threshold": 90,
        "dynamic_resharding": false,
        "epoch_length": 43200, //  <---------- EPOCH duration in seconds
        "fishermen_threshold": "340282366920938463463374607431768211455",
        "gas_limit": 1000000000000000,
        "gas_price_adjustment_rate": [
            1,
            100
        ],
        "genesis_height": 9820210,
        "genesis_time": "2020-07-21T16:55:51.591948Z",
        "max_gas_price": "10000000000000000000000",
        "max_inflation_rate": [
            0,
            1
        ],
        // ---- snip ----
}
```

您可以在[Validator FAQ](/docs/validator/staking-faq#what-is-an-epoch)中了解有关如何使用纪元来管理网络验证的更多信息。
> 有任何问题?
  <a href="https://stackoverflow.com/questions/tagged/nearprotocol">
  <h8>在StackOverflow上提问</h8></a>
