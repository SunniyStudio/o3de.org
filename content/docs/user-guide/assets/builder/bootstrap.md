---
linkTitle: 启动
title: 启动 Python Asset Builder
description: 在 bootstrap.py 脚本中添加 Python 资产创建器。
weight: 100
---

要使**Python 资产生成器**脚本可用于**资产流水线**，必须在路径中添加一个 `bootstrap.py` 文件或修改现有的 `bootstrap.py` 文件。建议将 Python Asset Builder 脚本和`bootstrap.py`文件放在以下脚本位置之一：

+ 在你的项目中：`Editor\Scripts\bootstrap.py`
+ 在Gem中：`Editor\Scripts\bootstrap.py`

要添加 Python 资产生成器，请在 `bootstrap.py` 文件中导入它。下面的示例假定 Python 资产生成器名为 `my_py_asset_builder.py`，并且与 `bootstrap.py` 文件位于同一目录。

```python
import my_py_asset_builder
```
