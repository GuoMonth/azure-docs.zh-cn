---
title: 在 Azure 机器学习中启用日志记录
description: 了解如何使用默认的 Python 日志记录包以及 SDK 特定的功能，在 Azure 机器学习中启用日志记录。
ms.author: larryfr
author: BlackMist
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.date: 03/05/2020
ms.topic: conceptual
ms.custom: how-to, tracking-python
ms.openlocfilehash: d4fb08a618c6dfb35f3f8d154af6820c92d62781
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87320130"
---
# <a name="enable-logging-in-azure-machine-learning"></a>在 Azure 机器学习中启用日志记录
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

借助 Azure 机器学习 Python SDK，可以使用默认的 Python 日志记录包以及 SDK 特定的功能启用日志记录，以便在门户中进行本地日志记录，以及对工作区进行日志记录。 日志可为开发人员提供有关应用程序状态的实时信息，并可以帮助诊断错误或警告。 本文介绍如何针对以下场景启用日志记录：

> [!div class="checklist"]
> * 训练模型和计算目标
> * 映像创建
> * 部署的模型
> * Python `logging` 设置

[创建 Azure 机器学习工作区](how-to-manage-workspace.md)。 有关 SDK 的详细信息，请使用[指南](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)。

## <a name="training-models-and-compute-target-logging"></a>训练模型和计算目标日志记录

在模型训练过程中，可通过多种方法启用日志记录，示例将演示常用设计模式。 可以使用 `Experiment` 类中的 `start_logging` 函数，将运行相关的数据轻松记录到云中的工作区。

```python
from azureml.core import Experiment

exp = Experiment(workspace=ws, name='test_experiment')
run = exp.start_logging()
run.log("test-val", 10)
```

有关其他日志记录函数，请参见 [Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py) 类的参考文档。

若要在训练过程中启用应用程序状态的本地日志记录，请使用 `show_output` 参数。 启用详细日志记录可以查看训练过程的详细信息，以及有关任何远程资源或计算目标的信息。 提交试验时使用以下代码启用日志记录。

```python
from azureml.core import Experiment

experiment = Experiment(ws, experiment_name)
run = experiment.submit(config=run_config_object, show_output=True)
```

还可以在生成的运行上的 `wait_for_completion` 函数中使用相同的参数。

```python
run.wait_for_completion(show_output=True)
```

在某些训练场景中，SDK 还支持使用默认的 Python 日志记录包。 以下示例在 `AutoMLConfig` 对象中启用日志记录级别 `INFO`。

```python
from azureml.train.automl import AutoMLConfig
import logging

automated_ml_config = AutoMLConfig(task='regression',
                                   verbosity=logging.INFO,
                                   X=your_training_features,
                                   y=your_training_labels,
                                   iterations=30,
                                   iteration_timeout_minutes=5,
                                   primary_metric="spearman_correlation")
```

创建持久计算目标时还可以使用 `show_output` 参数。 在 `wait_for_completion` 函数中指定参数可在创建计算目标期间启用日志记录。

```python
from azureml.core.compute import ComputeTarget

compute_target = ComputeTarget.attach(
    workspace=ws, name="example", attach_configuration=config)
compute.wait_for_completion(show_output=True)
```

## <a name="logging-for-deployed-models"></a>对部署的模型进行日志记录

若要从以前部署的 Web 服务检索日志，请加载该服务并使用 `get_logs()` 函数。 日志可以包含有关部署期间发生的任何错误的详细信息。

```python
from azureml.core.webservice import Webservice

# load existing web service
service = Webservice(name="service-name", workspace=ws)
logs = service.get_logs()
```

还可以通过启用 Application Insights 来记录 Web 服务的自定义堆栈跟踪，这样，便可以监视请求/响应时间、失败率和异常。 针对现有的 Web 服务调用 `update()` 函数可启用 Application Insights。

```python
service.update(enable_app_insights=True)
```

有关详细信息，请参阅[监视机器学习 Web 服务终结点并从中收集数据](how-to-enable-app-insights.md)。

## <a name="python-native-logging-settings"></a>Python 本机日志记录设置

SDK 中的某些日志可能包含一个错误，指示你将日志记录级别设置为“调试”。 若要设置日志记录级别，请在脚本中添加以下代码。

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

## <a name="next-steps"></a>后续步骤

* [监视 ML Web 服务终结点并从中收集数据](how-to-enable-app-insights.md)
