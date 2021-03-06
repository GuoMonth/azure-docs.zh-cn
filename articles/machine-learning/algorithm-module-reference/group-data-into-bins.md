---
title: 将数据分组到箱中：模块参考
titleSuffix: Azure Machine Learning
description: 了解如何使用“将数据分组到箱中”模块来对数字进行分组，或更改连续数据的分布。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/19/2020
ms.openlocfilehash: d3a9f88325f03d0252adf51c5bf221b131d7d33b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "84751719"
---
# <a name="group-data-into-bins-module"></a>“将数据分组到箱中”模块

本文介绍如何使用 Azure 机器学习设计器（预览版）中的“将数据分组到箱中”模块来对数字进行分组或更改连续数据的分布。

“将数据分组到箱中”模块支持使用多个选项将数据分箱。 可以自定义量化边界的设置方式，以及在箱中分配值的方式。例如，可以：  

+ 手动键入一系列要用作箱边界的值。  
+ 使用*分位数*或百分位排名将值分配到箱。  
+ 强制将值均匀分布到箱中。  

## <a name="more-about-binning-and-grouping"></a>有关分箱和分组的更多信息

在为机器学习准备数字数据时，将数据“分箱”或分组（有时称为“量化”）是一个重要工具 。 它适用于如下场景：

+ 一列连续的数字中的唯一值太多，无法有效地建模。 因此你会自动或手动将这些值分配到各组，以创建较小的一组离散范围。

+ 你需要将一列数字替换为表示特定范围的分类值。

    例如，对于用户人口统计数据，你可能想要在年龄列中通过指定自定义范围（例如 1-15、16-22、23-30 等）来对该列中的值分组。

+ 某个数据集包含几个极端值，这些值远远超出预期的范围，并且对训练的模型造成了很大的影响。 为了减少模型中的偏差，可通过使用分位数方法将数据转换为均匀分布。

    使用此方法时，“将数据分组到箱中”模块将确定理想的箱位置和箱宽度，以确保在每个箱中放入数量大致相同的样本。 然后，根据你选择的归一化方法，箱中的值将转换为百分位数或者映射到箱号。

### <a name="examples-of-binning"></a>分箱示例

下图显示了使用*分位数*方法分箱之前和之后的数字值分布。 请注意，与左侧的原始数据相比，数据已分箱并转换为单位法线标度。  

你可以找到[此管道运行的结果中的示例](https://ml.azure.com/visualinterface/authoring/Normal/87270db9-4651-448e-bd28-8ef7428084dc?wsid=%2Fsubscriptions%2Fe9b2ec51-5c94-4fa8-809a-dc1e695e4896%2Fresourcegroups%2Fmodule-ws-rg%2Fworkspaces%2Fmodule-prerelease-119&flight=cm&tid=72f988bf-86f1-41af-91ab-2d7cd011db47&smtendpoint=https%3A%2F%2Fsmt-test1.azureml-test.net)。

由于可以通过许多方式对数据进行分组且所有方式都可自定义，因此我们建议使用不同的方法和值进行试验。 

## <a name="how-to-configure-group-data-into-bins"></a>如何配置“将数据分组到箱中”

1. 在设计器（预览版）中将“将数据分组到箱中”模块添加到管道。 可以在“数据转换”类别中找到此模块。

2. 连接包含要分箱的数字数据的数据集。 量化只能应用于包含数字数据的列。 

    如果数据集包含非数字列，请使用[选择数据集中的列](select-columns-in-dataset.md)模块选择要处理的列子集。

3. 指定分箱模式。 分箱模式决定其他参数，因此请务必首先选择“分箱模式”选项。 以下分箱类型受支持：

    - **分位数**：分位数方法根据百分位排名将值分配到箱中。 此方法也称为等高分箱。

    - **等宽**：如果使用此选项，必须指定箱的总数。数据列中的值会放入箱中，且每个箱在开始值和结束值之间的间隔均相同。 因此，如果数据聚集在特定点附近，则一些箱可能会包含更多的值。

    - **自定义边界**：可以指定每个箱的开始值。 边界值始终是箱的下边界。 
    
      例如，假设要将值分组到两个箱中。其中一个箱中有大于 0 的值，另一个箱中有小于或等于 0 的值。 在这种情况下，对于量化边界，请在“量化边界的逗号分隔列表”中输入“0” 。 该模块的输出将会是 1 和 2，表示每个行值的箱索引。 请注意，逗号分隔值列表必须采用升序，例如“1,3,5,7”。

4. 如果使用“分位数”和“等宽”分箱模式，请使用“箱数”选项来指定要创建的箱数（或“分位数”）  。

5. 对于“要分箱的列”，请使用列选择器来选择要分箱的值所在的列。 列必须是数字数据类型。

    同一分箱规则将应用到你选择的所有适用列。 如果需要通过使用其他方法将某些列分箱，请对每组列都使用“将数据分组到箱中”模块的一个单独实例。

    > [!WARNING]
    > 如果选择的列不属于允许的类型，会生成运行时错误。 模块在发现任一列的类型不被允许时，会立即返回错误。 如果收到错误，请检查所有选定的列。 该错误不会列出所有无效列。

6. 对于“输出模式”，请指明输出量化值时需要使用的方式：

    + **追加**：创建一个包含分箱值的新列，并将其追加到输入表。

    + **Inplace**：使用数据集中的新值替换原始值。

    + **ResultOnly**：仅返回结果列。

7. 如果选择“分位数”分箱模式，请使用“分位数归一化”选项来确定值在按分位数排序之前如何归一化。  请注意，将值归一化会转换这些值，但不影响最终的箱数。

    以下归一化类型受支持：

    + **Percent**：值在 [0,100] 范围内归一化。

    + **PQuantile**：值在 [0,1] 范围内归一化。

    + **QuantileIndex**：值在 [1,箱数] 范围内归一化。

8. 如果选择“自定义边界”选项，请在“量化边界的逗号分隔列表”文本框中输入要用作量化边界的逗号分隔数字列表。 

    这些值标记了分隔各箱的点。例如，如果输入一个量化边界值，将会生成两个箱。 如果输入两个量化边界值，将会生成三个箱。

    这些值必须按箱的创建顺序从低到高排序。

10. 选择“将列标记为分类”选项可指明应将量化列作为分类变量进行处理。

11. 提交管道。

## <a name="results"></a>结果

“将数据分组到箱中”模块返回一个数据集，其中每个元素都已根据指定的模式分箱。 

它还返回“分箱转换”。 该函数可传递到“[应用转换](apply-transformation.md)”模块，以通过使用相同的分箱模式和参数将新的数据样本分箱。  

> [!TIP]
> 如果对训练数据使用分箱，就必须对数据使用在测试和预测时所用的相同分箱方法。 还必须使用相同的箱位置和箱宽度。 
> 
> 为确保始终使用相同的分箱方法转换数据，建议保存有用的数据转换。 然后通过使用“[应用转换](apply-transformation.md)”模块将这些转换应用到其他数据集。

## <a name="next-steps"></a>后续步骤

请参阅 Azure 机器学习的[可用模块集](module-reference.md)。 
