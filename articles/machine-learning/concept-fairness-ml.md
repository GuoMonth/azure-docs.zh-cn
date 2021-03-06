---
title: " (预览) 评估和缓解机器学习模型中的公平问题"
titleSuffix: Azure Machine Learning
description: 了解机器学习模型中的公平性以及 Fairlearn Python 包如何帮助你构建更公平的模型。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: luquinta
author: luisquintanilla
ms.date: 07/09/2020
ms.openlocfilehash: 2cc3228c20fba322ec804a3bcc9ee322c7d37907
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2020
ms.locfileid: "86207300"
---
# <a name="build-fairer-machine-learning-models-preview"></a> (预览生成 fairer 机器学习模型) 

了解机器学习和[Fairlearn](https://fairlearn.github.io/)的开源 Python 包如何帮助您构建更公平的模型。 如果你不想要了解公平问题，并在构建机器学习模型时评估公平，则可以构建产生不公平结果的模型。 

下面是 Fairlearn 开源包[用户指南](https://fairlearn.github.io/user_guide/index.html)的摘要，介绍如何使用它来评估你正在构建的 AI 系统的公平。  Fairlearn 开源包还可以提供一些选项，以帮助缓解或降低所观察到的任何公平问题。  请参阅[如何](how-to-machine-learning-fairness-aml.md)在 Azure 机器学习培训[sample notebooks](https://github.com/Azure/MachineLearningNotebooks/tree/master/contrib/fairness)期间启用 AI 系统的公平评估。


## <a name="what-is-fairness-in-machine-learning-systems"></a>什么是机器学习系统中的公平性？

>[!NOTE]
> 公平性是一个社会性的技术难题。 公平性的许多方面（例如公正和正当程序）没有通过量化的公平性指标进行捕获。 另外，许多量化的公平性指标无法同时得到满足。 Fairlearn 开源包的目标是使人类能够评估不同的影响和缓解策略。 最终，用户需要构建人工智能和机器学习模型，以便进行适合其方案的权衡。

人工智能和机器学习系统可能会表现出不公平的行为。 定义不公平行为的一种方法是按其损害或对人的影响来定义。 人工智能系统可以造成许多类型的损害。 有关详细信息，请参阅[Kate Crawford 的 NeurIPS 2017 主题](https://www.youtube.com/watch?v=fMym_BKWQzk)。

人工智能造成的两种常见损害是：

- 分配的损害： AI 系统扩展或 withholds 特定组的商机、资源或信息。 示例包括招聘、学校招生和贷款。在这些场景中，某个模型在特定人群中挑选优秀候选人的能力可能要强于在其他人群中进行挑选的能力。

- 对服务质量的损害：人工智能系统针对一个人群的工作质量没有针对另一个人群的工作质量好。 例如，语音识别系统针对女性的工作质量可能没有针对男性的工作质量好。

为了减少人工智能系统中的不公平行为，你必须评估并缓解这些损害。


## <a name="fairness-assessment-and-mitigation-with-fairlearn"></a>通过 Fairlearn 进行公平性评估和缓解

Fairlearn 是一个开源 Python 包，它使机器学习系统开发者能够评估其系统的公平性并缓解所看到的公平性问题。

Fairlearn 开源包包含两个组件：

- 评估仪表板：这是一个 Jupyter 笔记本小组件，用于评估模型的预测如何影响不同的群体。 它还支持使用公平性和性能指标对多个模型进行比较。
- 缓解算法：一组用于在二元分类和回归中缓解不公平性的算法。

借助这些组件，数据科学家和业务主管能够在公平性和性能之间进行权衡，并选择最适合其需求的缓解策略。

## <a name="fairness-assessment"></a>公平性评估
在 Fairlearn 开源包中，公平通过一种称为 "**组公平**" 的方法来进行概念处理，这会询问：哪些个人组面临着损害的风险？ 相关群体（也称为亚群体）是通过**敏感特征**或敏感特性定义的。 将敏感功能作为向量或名为的矩阵传递到 Fairlearn 开放源包中的估计器 `sensitive_features` 。 此术语的意思是，系统设计者在评估群体公平性时应该对这些特征敏感。 

需要注意的一点是，这些功能是否包含因专用数据而产生的隐私问题。 但是“敏感”并不意味着这些特征不应被用来进行预测。

>[!NOTE]
> 公平评估并非纯粹的技术运动。  Fairlearn 开源包可帮助你评估模型的公平，但不会为你执行评估。  Fairlearn 开源包有助于识别量化指标以评估公平，但开发人员还必须执行定性分析来评估其自己的模型的公平。  上面所述的敏感功能是此类定性分析的示例。     

在评估阶段，将通过差异指标对公平性进行量化。 **差异指标**能够以比率或差值的形式评估并比较模型在不同群体中的行为。 Fairlearn 开源包支持两类差异指标：


- 模型性能差异：这些指标集计算所选性能指标的值在不同子群体之间的差异。 示例包括：

  - 准确率差异
  - 错误率差异
  - 精度差异
  - 召回率差异
  - MAE 差异
  - 许多其他差异

- 选择率差异：此指标包含不同子群体之间的选择率差异。 此差异的一个示例是贷款批准率差异。 选择率是指每个分类中归类为 1 的数据点所占的比例（在二元分类中）或者指预测值的分布（在回归中）。

## <a name="unfairness-mitigation"></a>不公平性缓解

### <a name="parity-constraints"></a>奇偶校验约束

Fairlearn 开源包包括各种 unfairness 缓解算法。 这些算法支持对预测器行为的一组约束（称为**奇偶校验约束**或条件）。 奇偶校验约束要求预测器行为的某些方面在敏感特征所定义的群体（例如不同的种族）之间具有可比性。 Fairlearn 开源包中的缓解算法使用此类奇偶校验约束来减轻观察到的公平问题。

>[!NOTE]
> 减少模型中的 unfairness 意味着降低 unfairness，但这种技术缓解无法完全消除此 unfairness。  Fairlearn 开源包中的 unfairness 缓解算法可提供建议的缓解策略，以帮助减少机器学习模型中的 unfairness，但它们并不是完全消除 unfairness 的解决方案。  对于每个特定开发人员的机器学习模型，还应考虑其他奇偶校验约束或条件。 使用 Azure 机器学习的开发人员必须自行确定缓解措施，以充分消除其预期用途和部署机器学习模型中的任何 unfairness。  

Fairlearn 开源包支持以下类型的奇偶校验约束： 

|奇偶校验约束  | 目的  |机器学习任务  |
|---------|---------|---------|
|人口统计奇偶校验     |  缓解分配损害 | 二元分类、回归 |
|均等几率  | 诊断分配和服务质量损害 | 二元分类        |
|同等机会 | 诊断分配和服务质量损害 | 二元分类        |
|有界群体损失     |  缓解服务质量损害 | 回归 |



### <a name="mitigation-algorithms"></a>缓解算法

Fairlearn 开源包提供了后处理和缩减 unfairness 缓解算法：

- 减少：这些算法采用标准的黑色机箱机器学习估计器 (例如，LightGBM 模型) 并使用一系列重新加权的定型数据集生成一组重新训练模型。 例如，某一性别的申请者可能会被提高或降低权重，然后重新训练模型，降低不同性别群体之间的差异。 然后，用户可以选择一个模型，该模型在准确性（或其他性能指标）与差异之间提供最佳的权衡，这一权衡通常需要基于业务规则和成本计算。  
- 后期处理：这些算法采用现有分类器和敏感特征作为输入。 然后，它们将派生分类器的预测转换，以强制实施指定的公平性约束。 阈值优化的最大优势在于其简易性和灵活性，因为它不需要重新训练模型。 

| 算法 | 说明 | 机器学习任务 | 敏感特征 | 支持的奇偶校验约束 | 算法类型 |
| --- | --- | --- | --- | --- | --- |
| `ExponentiatedGradient` | [A Reductions Approach to Fair Classification](https://arxiv.org/abs/1803.02453)（公平分类的约简方法）中描述的公平分类的黑盒方法 | 二元分类 | 分类 | [人口统计奇偶校验](#parity-constraints)、[均等几率](#parity-constraints) | 约简 |
| `GridSearch` | [A Reductions Approach to Fair Classification](https://arxiv.org/abs/1803.02453)（公平分类的约简方法）中描述的黑盒方法| 二元分类 | 二进制 | [人口统计奇偶校验](#parity-constraints)、[均等几率](#parity-constraints) | 约简 |
| `GridSearch` | 一种黑盒方法，它通过[公平回归：量化的定义和基于约简的算法](https://arxiv.org/abs/1905.12843)中描述的用于有界群体损失的算法实现公平回归的网格搜索变体。 | 回归 | Binary | [有界群体损失](#parity-constraints) | 约简 |
| `ThresholdOptimizer` | 基于 [Equality of Opportunity in Supervised Learning](https://arxiv.org/abs/1610.02413)（监督式学习中的机会均等性）一文的后期处理算法。 此方法采用现有分类器和敏感特征作为输入，并派生分类器预测的单一转换，以强制实施指定的奇偶校验约束。 | 二元分类 | 分类 | [人口统计奇偶校验](#parity-constraints)、[均等几率](#parity-constraints) | 后处理 |

## <a name="next-steps"></a>后续步骤

- 了解如何使用不同的组件，方法是查看 Fairlearn 的[GitHub](https://github.com/fairlearn/fairlearn/)、[用户指南](https://fairlearn.github.io/user_guide/index.html)、[示例](https://fairlearn.github.io/auto_examples/notebooks/index.html)和[示例笔记本](https://github.com/fairlearn/fairlearn/tree/master/notebooks)。
- 了解[如何](how-to-machine-learning-fairness-aml.md)在 Azure 机器学习中启用机器学习模型的公平评估。
- 有关 Azure 机器学习中的其他公平评估方案，请参阅[示例笔记本](https://github.com/Azure/MachineLearningNotebooks/tree/master/contrib/fairness)。 
