---
linktitle: 验证错误
title: 如何使用验证工具进行 O3DE 提交
description: 使用验证工具的详细信息和说明。
toc: true
weight: 300
---

**Open 3D Engine （O3DE）** 早期预警系统是一种用于样式检查和基本静态分析的工具，用于在构建引擎和执行测试之前发现错误。它在管道开始构建之前运行，尝试在花费机器资源进行构建和测试之前快速拒绝无效更改，并最大限度地减少审查更改的人工工作量。

{{< note >}}
您访问此页面的最可能原因是为了帮助解决验证失败问题。要找到相关的修复程序，请首先查看标准输出日志！
{{< /note >}}

## 什么文件失败了？什么规则失败了？

每次验证失败都会将一条消息记录到标准输出中。 搜索`FAILED`并查找表单的行：

```
<path_to_source_file>::ValidatorRuleName FAILED - <failure details>

# Example Output
VALIDATOR_FAILED: CopyrightHeaderValidator <file>::CopyrightHeaderValidator FAILED - Source file missing copyright headers.
```

## 如何提交修复？

修改您的代码以使其合规，然后重新提交到 [Automated Review](https://github.com/o3de/sig-build/tree/main/AutomatedReview)  管道。 如果您不想等待或不想调试验证器，您也可以在本地运行验证器。

如果验证器脚本包含 bug，建议您打开 [GitHub Issues 票证](https://github.com/o3de/sig-testing/issues/new/choose).

## 本地验证

可以通过运行位于`scripts/commit_validation`中的脚本在本地检查文件。

### Git 提交

检查 Git 提交中的所有更改，包括运行特定于 Git 的检查。

```cmd
python\python.cmd scripts\commit_validation\git_validate_branch.py --source <source branch> --target <target branch>
```

### 定位任何文件或文件夹树

验证器能够检查任意本地文件。为此，验证程序会模拟文件夹中的每个文件（或您指定的一个文件）都是您提议的更改的一部分，并更改文件中的每一行。这会导致所有验证器根据需要进行扫描。验证器不接受通配符。指定文件名或文件夹名称，它将递归运行。

```cmd
python\python.cmd scripts\commit_validation\validate_file_or_folder.py --path <path to validate>
```

## 管道验证

将此工具连接到持续集成系统与本地使用相同！ 只需在管道以非零代码退出时使管道失败即可。

O3DE 自动审阅提交管道使用 Jenkins 来管理此工具。

## 创建一个新的验证人

验证器是通过在`scripts/commit_validation/commit_validation/validators`中添加新类来创建的。

每个验证者都需要满足以下条件：

1. 继承自 CommitValidator 并实现 run（self， commit）。
2. 实现 get_validator（） 并返回新类。
