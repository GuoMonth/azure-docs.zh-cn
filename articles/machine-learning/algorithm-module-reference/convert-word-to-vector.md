---
title: 将单词转换为矢量：模块参考
titleSuffix: Azure Machine Learning
description: 了解如何使用提供的三个 Word2Vec 模型从文本语料库中提取词汇表及其对应的单词嵌入。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/19/2020
ms.openlocfilehash: 21b207ece1a2a7fd6f218716912d4c4d2c2f1ee2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "84753890"
---
# <a name="convert-word-to-vector-module"></a>“将单词转换为矢量”模块

本文介绍如何使用 Azure 机器学习设计器（预览版）中的“将单词转换为矢量”模块执行以下任务：

- 将各种 Word2Vec 模型（Word2Vec、FastText、GloVe 预训练模型）应用到指定为输入的文本语料库。
- 生成具有单词嵌入的词汇表。

此模块使用 Gensim 库。 有关 Gensim 的详细信息，请查看其[官方网站](https://radimrehurek.com/gensim/apiref.html)，该网站提供了教程和算法说明。

### <a name="more-about-converting-words-to-vectors"></a>详细了解如何将单词转换为矢量

一般来说，将单词转换为矢量（也称单词矢量化）是一种自然语言处理 (NLP) 过程。 该过程使用语言模型或技术将单词映射到矢量空间，即通过实数的矢量表示每个单词。 同时，它允许含义相似的单词使用相似的表示形式。

单词嵌入可用作 NLP 下游任务（例如，文本分类和情绪分析）的初始输入。

单词嵌入技术多种多样，在此模块中，我们实现三种广泛使用的方法。 两种（Word2Vec 和 FastText）是在线训练模型。 另一种是预训练模型，即 glove-wiki-gigaword-100。 

在线训练模型针对输入数据进行训练。 对于较大的文本语料库（例如，维基百科、Google 新闻），预先训练模型以脱机的模式进行脱机训练，通常包含大约100000000000个字。 单词嵌入在单词矢量化期间保持不变。 预训练单词模型可带来减少训练时间、更好地对单词矢量进行编码以及提高整体性能等好处。

下面是有关方法的一些信息：

+ Word2Vec 是使用浅层神经网络学习单词嵌入的最流行方法之一。 以下论文讨论了相关理论，可供你以 PDF 格式下载：[Efficient Estimation of Word Representations in Vector Space, by Mikolov, Tomas, et al](https://arxiv.org/pdf/1301.3781.pdf)（高效估算矢量空间中的单词表示形式，作者：Mikolov、Tomas 等）。此模块中的实现基于[适用于 Word2Vec 的 Gensim 库](https://radimrehurek.com/gensim/models/word2vec.html)。

+ 以下论文对 FastText 理论进行了说明，可供你以 PDF 格式下载：[Enriching Word Vectors with Subword Information, by Bojanowski, Piotr, et al](https://arxiv.org/pdf/1607.04606.pdf)（使用子词信息扩充单词矢量，作者：Bojanowski、Piotr 等）。此模块中的实现基于[适用于 FastText 的 Gensim 库](https://radimrehurek.com/gensim/models/fasttext.html)。

+ GloVe 预训练模型是 glove-wiki-gigaword-100。 它是基于维基百科文本语料库的预训练矢量的集合，其中包含 56 亿个令牌和 400,000 个无大写的词汇。 下面是可供你以 PDF 格式下载的文章：[GloVe:Global Vectors for Word Representation](https://nlp.stanford.edu/pubs/glove.pdf)（GloVe：单词表示形式的全局矢量）。

## <a name="how-to-configure-convert-word-to-vector"></a>如何配置将单词转换为矢量

此模块需要包含文本列的数据集。 经过预处理的文本更合适。

1. 将“将单词转换为矢量”模块添加到管道中。

2. 请提供包含一个或多个文本列的数据集作为模块的输入。

3. 对于“目标列”，请仅选择包含要处理的文本的一列。

    因为此模块根据文本创建词汇表，所以列的内容会各不相同，导致词汇表的内容也有所不同。 这是模块仅接受一个目标列的原因。

4. 对于“Word2Vec 策略”，请从“GloVe 预训练英语模型”、“Gensim Word2Vec”和“Gensim FastText”中进行选择。  

5. 如果“Word2Vec 策略”为“Gensim Word2Vec”或“Gensim FastText” ，请执行以下操作：

    + 对于“Word2Vec 训练算法”，请从“Skip_gram”和“CBOW”中进行选择。   [原始论文 (PDF)](https://arxiv.org/pdf/1301.3781.pdf) 中介绍了两者的差异。

        默认方法是 **Skip_gram**。

    + 对于“单词嵌入长度”，请指定单词矢量的维数。 此设置对应于 Gensim 中的 `size` 参数。

        默认嵌入大小为 100。

    + 对于“上下文窗口大小”，请指定要预测的单词和当前单词之间的最大距离。 此设置对应于 Gensim 中的 `window` 参数。

        默认窗口大小为 5。

    + 对于“epoch 数”，请指定语料库的 epoch（迭代）次数。 此设置对应于 Gensim 中的 `iter` 参数。

        默认 epoch 数为 5。

6. 对于“最大词汇表大小”，请指定生成的词汇表中的最大单词数。

    如果有超出此数量的非重复单词，则删除不常用的单词。

    默认词汇表大小为 10,000。

7. 对于“最小单词计数”，请提供最小单词计数。 模块会忽略频率低于此值的所有单词。

    默认值为 5。

8. 提交管道。

## <a name="examples"></a>示例

模块有一个输出：

+ **具有嵌入的词汇表**：包含生成的词汇表以及每个单词的嵌入。 一个维度占用一列。

以下示例演示了“将单词转换为矢量”模块的工作原理。 它将此模块（采用默认设置）应用于 Azure 机器学习（预览版）中提供的经过预处理的维基百科 SP 500 数据集。

### <a name="source-dataset"></a>源数据集

该数据集包含一个类别列，以及从维基百科提取的全文。 此表仅显示几个典型示例。

|文本|
|----------|
|nasdaq 100 component s p 500 component foundation founder location city apple campus 1 infinite loop street infinite loop cupertino california cupertino california location country united states...|
|br nasdaq 100 nasdaq 100 component br s p 500 s p 500 component industry computer software foundation br founder charles geschke br john warnock location adobe systems...|
|s p 500 s p 500 component industry automotive industry automotive predecessor general motors corporation 1908 2009 successor...|
|s p 500 s p 500 component industry conglomerate company conglomerate foundation founder location city fairfield connecticut fairfield connecticut location country usa area...|
|br s p 500 s p 500 component foundation 1903 founder william s harley br arthur davidson harley davidson founder arthur davidson br walter davidson br william a davidson location...|

### <a name="output-vocabulary-with-embeddings"></a>具有嵌入的输出词汇表

下表包含以维基百科 SP 500 数据集为输入的此模块的输出。 最左侧的列表示词汇表， 其嵌入矢量由同一行中其余列的值表示。

|词汇|嵌入维度 0|嵌入维度 1|嵌入维度 2|嵌入维度 3|嵌入维度 4|嵌入维度 5|...|嵌入维度 99|
|-------------|-------------|-------------|-------------|-------------|-------------|-------------|-------------|-------------|
|nasdaq|-0.375865|0.609234|0.812797|-0.002236|0.319071|-0.591986|...|0.364276
|组件|0.081302|0.40001|0.121803|0.108181|0.043651|-0.091452|...|0.636587
|s|-0.34355|-0.037092|-0.012167|0.151542|0.601019|0.084501|...|0.149419
|p|-0.133407|0.073244|0.170396|0.326706|0.213463|-0.700355|...|0.530901
foundation|-0.166819|0.10883|-0.07933|-0.073753|0.262137|0.045725|...|0.27487
founder|-0.297408|0.493067|0.316709|-0.031651|0.455416|-0.284208|...|0.22798
location|-0.375213|0.461229|0.310698|0.213465|0.200092|0.314288|...|0.14228
city|-0.460828|0.505516|-0.074294|-0.00639|0.116545|0.494368|...|-0.2403
apple|0.05779|0.672657|0.597267|-0.898889|0.099901|0.11833|...|0.4636
campus|-0.281835|0.29312|0.106966|-0.031385|0.100777|-0.061452|...|0.05978
infinite|-0.263074|0.245753|0.07058|-0.164666|0.162857|-0.027345|...|-0.0525
loop|-0.391421|0.52366|0.141503|-0.105423|0.084503|-0.018424|...|-0.0521

在此示例中，我们使用了默认的“Gensim Word2Vec”作为“Word2Vec 策略”，并且“训练算法”为“Skip-gram”。    “单词嵌入长度”为 100，因此我们有 100 个嵌入列。

## <a name="technical-notes"></a>技术说明

本部分包含常见问题的提示和解答。

+ 在线训练模型和预训练模型之间的差异：

    在这个“将单词转换为矢量”模块中，我们提供了三种不同的策略：两种在线训练模型和一种预训练模型。 在线训练模型将输入数据集用作训练数据，并在训练过程中生成词汇表和单词矢量。 预训练模型已通过大得多的文本语料库（例如维基百科或 Twitter 文本）进行训练。 预训练模型实际上是单词/嵌入对的集合。  

    如果选择 GloVe 预训练模型作为单词矢量化策略，它会从输入数据集汇总词汇表，并为预训练模型中的每个单词生成嵌入矢量。 使用预训练模型时无需在线训练，可以节省训练时间。 预训练模型的性能更好，尤其是在输入数据集大小相对较小的情况下。

+ 嵌入大小：

    一般而言，将单词嵌入长度设置为几百（例如，100、200、300）可获得良好的性能。 原因在于，较小的嵌入大小意味着较小的矢量空间，这可能会导致单词嵌入冲突。  

    对于预训练模型，单词嵌入长度是固定的。 在此实现中，glove-wiki-gigaword-100 的嵌入大小为 100。


## <a name="next-steps"></a>后续步骤

请参阅 Azure 机器学习的[可用模块集](module-reference.md)。 

如需特定于设计器（预览版）模块的错误列表，请参阅[机器学习错误代码](designer-error-codes.md)。
