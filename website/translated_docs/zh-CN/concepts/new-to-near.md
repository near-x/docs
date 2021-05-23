---
id: new-to-near
title: New to NEAR?
sidebar_label: New to NEAR?
---
# NEAR新用户？

欢迎加入！本页是方便您了解Near平台的指南。

如果您在此过程中有任何问题，加入我们[Discord](http://near.chat/)的社区，我们会为您提供帮助的。

## Near是什么？

Near协议(“NEAR"）是一个去中心化的开发平台，开发人员可以托管无服务器的应用程序和智能合约，可以轻松连接到“开放金融”[1]网络，并从“开放网络”[2]组件的生态系统中受益。

与大多数基于区块链的平台不同，NEAR Protocol的创建，是致力于为开发者和其终端用户提供世界上最简单且兼具他们所需的可扩展性与安全性的平台。具体地说，NEAR的设计是为了让以下事情变得更容易：

1. **构建** 构建去中心化的应用程序，即使您只习惯于构建“传统”概念的网络或应用程序。
2. **平台** 用户可以在平台获得平稳流畅的体验，即使他们从未使用过加密、通证、密钥、钱包或其他区块链产品。
3. **扩展** 无缝扩展应用程序——底层平台通过分片自动扩容，无需额外的成本或努力。

[1]: *"开放 金融"网络使用代币和代币化资产促进数字价值的转移和存储，这涉及从简单的点对点支付到复杂的借贷和交易协议的方方面面。*

[2]: *"开放 网络"组件是可重复使用、共享状态的智能合约，使应用程序易于组合，同时保护用户数据。而开放金融是建立在无需许可的价值流动之上的，开放网络则进一步将这种开放性推广到所有数据的操作上。*

## 区块链是什么?

区块链是一种特定类型的不可逆的分布式账本，它结合了计算和数据存储两个方面。每个新增加的区块都包含对分类账状态的修改，这些修改已得到运行网络的所有分布式节点的一致同意。  

这些分类账允许大量参与者在无需许可的情况下，通过基本的加密经济激励来协同管理极其巨大的价值(比特币的价值为>$1000亿)。

尽管从理论上讲探索区块链背后的理论和技术很有趣，但是并不需要为了构建，测试和部署应用而这样做。正如你不需要了解在AWS、GCP或Azure中容错计算集群是如何工作的，就可以将应用程序部署到这些云上一样。请专注代码!我们让它变得非常简单了。

## 为什么我们要创建Near？

您可能听说过分布式计算、数据库或计算机网络，所有这些都在区块链中发挥着作用。 

目前，大多数网络服务使用单个服务器和单个数据库来处理用户请求以及提供信息。这个基础设施通常由一个单个实体管理，而该实体将其所有数据处理视为一个黑盒：请求进入，发生一些事情，然后用户接收到输出。

虽然该公司可能会依赖第三方来核实这些说法，但用户永远无法验证黑盒子中发生了什么。这个体系依赖于用户与公司之间的信任。

Near基本上是类似于“基于云计算”的基础设施，让开发者在上面开发应用程序。不同之处在于，Near不再是由单个公司运行控制的一个巨大数据中心，数据中心实际上是由全世界所有在分散网络上运维节点的人组成。它再不是“公司运营的云”，而是“社区运营的云”。 

为此，我们正在构建一个“底层区块链”，或者说是第一层，这意味着它与以太坊或Cosmos等项目处于同一个生态级别，其生态系统中的所有生态内容都是构建在NEAR区块链之上的，包括您的应用程序。

## 最佳定向视频

- [ [观看](https://www.youtube.com/watch?v=Y21YtLzGbH0&feature=youtu.b&t=2656) ] 区块链101入口:解构区块链生态系统
- [ [观看](https://www.youtube.com/watch?v=Gd-aNfDqgQY&feature=youtu.be&t=1100) ] 什么是去中心化应用程序，它们是如何工作的?
- [ [观看](https://www.youtube.com/watch?v=Y21YtLzGbH0&feature=youtu.b&t=2656) ] 基于区块链的应用程序设计
- [ [观看](https://www.youtube.com/watch?v=bBC-nXj3Ng4) ]比特币实际上是如何工作的?作者：3blue1brown

### 最佳定向资源

- [ [阅读](https://near.org/blog/the-beginners-guide-to-the-near-blockchain/) ]《接近区块链的初学者指南》
- [ [阅读](https://medium.com/@trentmc0/blockchain-infrastructure-landscape-a-first-principles-framing-92cc5549bafe) ] 区块链基础设施景观:第一原则框架
- [ [阅读](https://a16z.com/2019/11/08/crypto-glossary/) ] a16z加密术语表
- [ [阅读](https://a16z.com/2018/02/10/crypto-readings-resources/) ] a16z密码标准

## 该如何开始呢？

1. 创建一个 [账户](https://wallet.testnet.near.org/) 。
2. 选择一个 [启动项目](http://near.dev/)，点击顶部的 `运行`，然后体验几分钟。
3. 检查 [网络状态](http://explorer.testnet.near.org) （以及在第2步中你所做的任何变动），区块链浏览器为您提供了对节点、交易和块的洞察。您可以查找您的账户ID（在步骤2中使用过的）。
4. 深入 [研究这些文档](https://docs.near.org) 。
5. 任何需要都可以 [告诉我们](http://near.chat)。

### 有什么需要事先知道的吗？

在基于区块链的分片平台上做开发，概念上类似于构建web应用程序，但仍有一些差异需要注意。例如：支持这些应用程序的“智能合约”，要求在部署到生产环境时仔细考虑良好的安全实践、异步调用和发布管理。

幸运的是，这些文档中有很多可用的工具，可以用来测试并了解他们的工作原理。

## 还能探索些什么？

### 网络状态

[ [打开](https://nearprotocol.statuspal.io/) ]NEAR Protocol网络状态页面

### 午餐会系列

[ [观看](https://www.youtube.com/watch?v=mhJXsOKoSdg&list=PL9tzQn_TEuFW_t9QDzlQJZpEQnhcZte2y) ] 定期发表的新专辑
- NEAR午餐会Ep.05： **帐户和运行环境**
- NEAR午餐会Ep.04： **夜影：共识与定局**
- NEAR午餐会Ep.03： **POS(权益证明)系统中的轻客户端**
- NEAR午餐会Ep.02： **分片区块链中的经济学**
- NEAR午餐会Ep.01： **有一个块延迟的跨分片交易**

### 白板系列

[ [观看](https://www.youtube.com/playlist?list=PL9tzQn_TEuFWweVbfTbaedFdwVrvaYPq4) ] 定期发表的新专辑
- NEAR白板系列 | Ep:31 Kava实验室 **Kevin Davis**
- NEAR白板系列 | Ep:30 Sia联合创始人 **David Vorick**
- NEAR白板系列 | Ep:29 Top Network创始人 **Taylor Wei**
- NEAR白板系列 | Ep:28 Matic Network首席执行官 **Jaynti Kanani**
- NEAR白板系列 | Ep:27 Meter创始人 **祝小翰**

### StackOverflow问题

[ [视图](https://stackoverflow.com/tags/nearprotocol) ]  定期发布的新问题及答案

- 我们是否可以认为基于web的非插件加密钱包是安全的?([视图](https://stackoverflow.com/questions/59165184/could-we-consider-non-plugins-web-based-crypto-wallets-as-safe))
- 如何在AssemblyScript/Near中打印数组的长度？([视图](https://stackoverflow.com/questions/57897731/how-to-print-the-length-of-an-array-in-assemblyscript-near))
- 在测试过程中改变VMContext属性([视图](https://stackoverflow.com/questions/58956740/changing-vmcontext-attributes-during-tests))
- 用初始化方法设置字符串属性总是返回空字符串([视图](https://stackoverflow.com/questions/58659873/string-attribute-set-in-init-method-always-returns-empty-string))
- 如何在near-api-js的交易中附加价值(押金)?([视图](https://stackoverflow.com/questions/57904221/how-to-attach-value-deposit-to-transaction-with-near-api-js))

### 联系我们

- [ [订阅](https://near.org/newsletter) ]我们的电子期刊
- [ [加入](https://near.org/events/) ]我们即将举行的活动
- [ [阅读](https://near.org/blog/) ]我们的博客

>有问题吗？
<a href="https://stackoverflow.com/questions/tagged/nearprotocol">
在StackOverflow上提问吧！ </a>