---
title: 使用Resource Mapping Tool
description: 了解如何在 Open 3D Engine 中使用 AWS Resource Mapping Tool。
weight: 350
toc: true
---

资源映射工具有助于管理和配置您的资源映射文件。

## 先决条件

资源映射工具使用 boto3 与后端 AWS 服务进行交互。在使用该工具之前，您需要执行以下操作：

1. 设置您的 AWS 凭证。有关帮助，请参阅 [配置 AWS 凭证](./configuring-credentials/)。
1. 设置 AWS Core [项目设置](./getting-started/#project-settings)以配置 AWS 配置文件名称（如果使用配置文件）。

## 从 Editor 启动工具

最简单的方法是从 O3DE 编辑器启动该工具。使用 **AWS - AWS Resource Mapping tool** 菜单项启动该工具。

该工具也可以独立于 Editor 运行。有关说明，请参阅 [从命令行启动资源映射工具](#launching-the-resource-mapping-tool-from-command-line)部分。

## 导入资源

执行以下操作，将资源导入到项目的资源映射文件中。

### 1. 选择映射文件

选择要编辑的现有映射文件，或生成新的映射文件。

**生成新的映射文件**

在新项目中首次启动时，不存在资源映射文件。

![Start a new mapping file using the resource mapping tool](/images/user-guide/gems/reference/aws/aws-core/resource-mapping-new.png)

选择 **Create New** 以生成新的 映射 文件。然后使用 **Config File** 下拉列表选择刚刚创建的文件。

**打开现有映射文件**

使用 **Config File** 下拉列表选择<Project>\Config中的现有映射文件。

### 2. 导入资源

您可以单独添加或导入资源，也可以从 AWS CloudFormation 堆栈导入资源。

#### Importing individual resources导入单个资源

![Use Import Additional Resources to import AWS resources in the resource mapping tool](/images/user-guide/gems/reference/aws/aws-core/resource-mapping-import.png)

**导入账户中部署的资源**

1. 打开 **Import Additional Resources**。
1. 选择 **Import AWS Resources**。
1. 选择要导入的资源类型。
1. 选择 **Search**。
1. 选择要导入的资源。
1. 设置键名称。
1. 选择 **Save Changes** 保存文件。

**将资源添加到映射文件**

1. 选择 **Add Row**。
1. 添加映射键名称。
1. 输入资源类型。类型应该是 **AWS CloudFormation** 类型。例如：`AWS::Lambda::Function`。
1. 提供项目的 Name （名称） 或 ID，例如 Lambda 函数名称。
1. 选择 **Save Changes**。

#### 从 AWS CloudFormation 堆栈导入

**从现有堆栈导入资源**

1. 打开 **Import Additional Resources**。
1. 选择 **Import CFN Stacks**.
1. 选择 **Search**来描述您账户中的堆栈。
1. 选择要导入的资源。
1. 导入资源。
1. 设置资源的键名称。
1. 选择 **Save Changes** 以保存文件。

## 从命令行启动资源映射工具

对于使用哪个 Python 运行时，您有两个选项 - 您自己的运行时，或 O3DE 附带的运行时。

### 工具参数
* `--binaries-path` **[Optional]** PySide 所需的 QT 二进制文件的路径，如果使用引擎 python 环境启动工具，则需要。
* `--config-path`   **[Optional]** 路径到资源映射配置目录，如果没有提供，工具将使用当前目录。
* `--debug`         **[Optional]** Execute on debug （在调试模式下执行） 以启用 DEBUG 日志记录级别。
* `--log-path`      **[Optional]** 资源映射工具日志目录的路径，如果没有提供，工具会将日志存储在工具源代码目录下。
* `--profile`       **[Optional]** 用于查询 AWS 资源的命名 AWS 配置文件，如果未提供，工具将使用“`default`”aws 配置文件。

### 选项 1: 设置你自己的Python虚拟环境

此项目的设置与标准 Python 项目类似。初始化过程还会在此项目中创建一个“`virtualenv`”虚拟环境，存储在“`.venv`”目录下。要创建 '`virtualenv`'，你的路径中必须有一个 '`python3`' 可执行文件（或 Windows 的 '`python`'），可以访问 '`venv`' 包。如果由于任何原因自动创建 '`virtualenv`' 失败，你可以手动创建 '`virtualenv`'。

打开资源映射工具目录的命令提示符。

```cmd
cd <ENGINE_ROOT>\Gems\AWSCore\Code\Tools\ResourceMappingTool
```

创建一个`virtualenv`。

```cmd
# Windows
python -m venv .venv

# Mac or Linux
python3 -m venv .venv
```

激活你的`virtualenv`。

```cmd
# Windows
.venv\Scripts\activate.bat

# Mac or Linux
source .venv/bin/activate
```

安装所需的依赖项。

```cmd
# Windows
pip install -r requirements.txt

# Mac or Linux
pip3 install -r requirements.txt
```

启动位于引擎根目录中的资源映射工具。

```cmd
python resource_mapping_tool.py
```

### 选项 2: 使用O3DE中的Python发布版

此选项需要构建您的项目和 O3DE 编辑器。有关详细信息，请参阅位于`<ENGINE_ROOT>\Gems\AWSCore\Code\Tools\ResourceMappingTool`中的`README/` 中的说明。

打开命令提示符并将目录更改为引擎根目录。

```cmd
cd <ENGINE_ROOT>
```

启动资源映射工具，指定项目的构建文件夹和您选择的构建配置的路径。

```cmd
python\python.cmd Gems\AWSCore\Code\Tools\ResourceMappingTool\resource_mapping_tool.py --binaries_path <PATH_TO_BUILD_FOLDER>\bin\<BUILD_CONFIGURATION>\AWSCoreEditorQtBin
```

使用 **profile** 构建配置的示例：

```cmd
python\python.cmd Gems\AWSCore\Code\Tools\ResourceMappingTool\resource_mapping_tool.py --binaries_path C:\MyProject\bin\profile\AWSCoreEditorQtBin
```

## 故障排除

### 检查日志
检查您在启动工具时提供的`--log-path` 中生成的资源映射工具日志`resource_mapping_tool.log`。如果未提供`--log-path`，则会在工具源码目录下生成日志文件。

如果从 Editor 启动工具，则会在`<project-directory>/user/logs/resource_mapping_tool.log`中生成日志文件

### 无法调用资源
检查 [资源映射文件](/docs/user-guide/gems/reference/aws/aws-core/resource-mapping-files/)是否配置正确。确保查找名称按预期定义，并且它映射到预期的属性。

日志中 ```Cannot determine region from the url```  形式的错误表示为资源设置了错误的区域或帐户。

* 如果覆盖全局区域和账户属性，请仔细检查资源的映射条目是否具有正确的区域和账户信息。
* 仔细检查资源的映射条目是否具有正确的资源类型和名称。
* 确保映射文件为区域和版本设置所需的全局属性。确保它们设置为预期值。
