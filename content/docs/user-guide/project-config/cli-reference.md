---
linktitle: O3DE CLI 参考
title: O3DE 项目配置 CLI 参考
description: Open 3D Engine （O3DE） Python 脚本的命令行界面 （CLI） 参考。
weight: 200
toc: true
---

使用`o3de` Python 脚本从命令行界面 （CLI） 配置 O3DE 环境及其对象，包括引擎、项目和 Gem。该脚本聚合了 Open 3D Engine （O3DE） 中较低级别脚本的所有函数，因此您可以从一个点访问它们。在 engine 目录的 `/scripts/o3de.py`中找到该脚本。

您可以使用`o3de` Python 脚本执行以下任务：

- [创建项目](/docs/welcome-guide/create/creating-projects-using-cli/)
- 创建 Gem
- 创建模板
- [为您的项目启用和禁用 Gem](add-remove-gems/#using-the-command-line-interface-cli)
- 注册引擎
- 注册项目和 Gem
  

<!-------------------------------------------------------------->

## 先决条件

要使用`o3de` Python 脚本，您必须设置 Python 运行时，您可以在设置引擎时下载该运行时。有关下载运行时的说明，请参阅适用于[Windows](/docs/welcome-guide/setup/setup-from-github/building-windows/#register-the-engine) 或 [Linux](/docs/welcome-guide/setup/setup-from-github/building-linux/#register-the-engine) 的从 GitHub 设置 O3DE 主题的**注册引擎**部分。

<!-------------------------------------------------------------->

## 快速开始

{{< tabs name="Python runtime setup" >}}
{{% tab name="Windows" %}}

要运行 O3DE Python 运行时和命令，请在命令提示符处执行以下任一操作：

- 直接运行`o3de.bat` 批处理文件。
    ```cmd
    <engine>\scripts\o3de.bat <command>
    ```

- 启动 Python 并运行`o3de.py`脚本。
    ```cmd
    <engine>\python\python.cmd <engine>\scripts\o3de.py <command>
    ```

{{< note >}}
将`<engine>`替换为引擎的路径。
{{< /note >}}

例如，要注册引擎，请输入以下命令：

```cmd
<engine>\scripts\o3de.bat register --this-engine
```

{{% /tab %}}
{{% tab name="Linux" %}}

要在终端窗口中运行 O3DE Python 运行时和命令，请执行以下任一操作：

- 直接运行`o3de.sh` shell 脚本文件。
    ```bash
    <engine>/scripts/o3de.sh <command>
    ```

- 启动 Python 并运行 `o3de.py` 脚本。
    ```bash
    <engine>/python/python.sh <engine>/scripts/o3de.py <command>
    ```

{{< note >}}
将 `<engine>` 替换为引擎的路径。
{{< /note >}}

例如，要注册引擎，请输入以下命令：

```bash
<engine>/scripts/o3de.sh register --this-engine
```

{{% /tab %}}
{{< /tabs >}}


<!-------------------------------------------------------------->

## 命令

`o3de` Python 脚本包含以下命令，以下部分提供了更多详细信息。

| 命令 | 说明 | 
| - | - |
| [`get-global-project`](#get-global-project) | 获取注册到引擎的全局项目。 |
| [`set-global-project`](#set-global-project) | 将指定的项目设置为引擎的全局项目。 |
| [`create-template`](#create-template) | 从指定的源路径创建模板。 |
| [`create-from-template`](#create-from-template) | 从类属模板创建实例。  |
| [`create-project`](#create-project) | 创建新项目。 |
| [`create-gem`](#create-gem) | 创建新 Gem。 |
| [`register`](#register) | 注册 O3DE 对象。 |
| [`register-show`](#register-show) | 显示已注册的 O3DE 对象。 |
| [`get-registered`](#get-registered) | 显示具有指定名称的已注册 O3DE 对象的路径。 |
| [`enable-gem`](#enable-gem) | 在项目中启用/激活 Gem。 |
| [`disable-gem`](#disable-gem) | 禁用/停用项目中的 Gem。 |
| [`edit-engine-properties`](#edit-engine-properties) | 编辑引擎的属性。 |
| [`edit-project-properties`](#edit-project-properties) | 编辑项目的属性。 |
| [`edit-gem-properties`](#edit-gem-properties) | 编辑 Gem 的属性。 |
| [`download`](#download) | 从远程存储库下载内容。 |
| [`repo`](#repo) | 激活和停用远程存储库并更新元数据。 |
| [`sha256`](#sha256) | 使用 SHA-256（安全哈希算法 256）为 O3DE 对象创建哈希值。 |
| [`android-configure`](#android-configure) | 配置 Android Project Generation 脚本的设置。 |
| [`android-generate`](#android-generate) | 为您的项目生成 Android Gradle 项目脚本。 |

<!-------------------------------------------------------------->

## `get-global-project`

获取注册到引擎的全局项目。默认情况下，从`<USER_DIRECTORY>/.o3de/Registry/bootstrap.setreg`读取全局项目。您还可以使用 `-i` 指定文件路径。

### 格式

```cmd
get-global-project [-h] [-i INPUT_PATH]
```

### 用法

从 `<USER_DIRECTORY>/.o3de/Registry/bootstrap.setreg` 读取全局项目。

```cmd
o3de.bat get-global-project
```

从指定文件中读取全局项目。

```cmd
o3de.bat get-global-project -i <USER_DIRECTORY>\.o3de\Registry\my-custom.setreg
```

### 可选参数

- **`-h, --help`**

  显示帮助消息。

- **`-i INPUT_PATH, --input-path INPUT_PATH`**

  要从中读取 `/Amazon/AzCore/Bootstrap/project_path` 键的输入文件的路径。如果未提供，则此命令使用输入文件 `<USER_DIRECTORY>/.o3de/Registry/bootstrap.setreg`  代替。

<!-------------------------------------------------------------->

## `set-global-project`

将指定的项目设置为引擎的全局项目。默认情况下，在`<USER_DIRECTORY>\.o3de\Registry\bootstrap.setreg`中设置它。您还可以使用 `-o` 指定文件路径。

如果您设置了全局项目，则当您从已安装的引擎启动 O3DE 工具（例如 **Asset Processor** 和 **O3DE Editor**）时，它们将使用全局项目。要覆盖全局项目，请在从命令行启动 O3DE 工具时使用 `project-path` 参数指定项目。

### 格式 

```cmd
set-global-project [-h] (-pp PROJECT_PATH | -pn PROJECT_NAME)
                                  [-o OUTPUT_PATH] [-f]
```

### 用法

将指定的项目设置为 `<USER_DIRECTORY>/.o3de/Registry/bootstrap.setreg` 中的全局项目。

```cmd
o3de.bat set-global-project -pp PROJECT_PATH
```

将指定的项目设置为指定路径中的全局项目。

```cmd
o3de.bat set-global-project -pp PROJECT_PATH -o <USER_DIRECTORY>\.o3de\Registry\my-custom.setreg
```

### 可选

- **`-h, --help`**

  显示帮助消息。

- **`-pp PROJECT_PATH, --project-path PROJECT_PATH`**

  项目的路径。该路径必须包含项目清单文件(`project.json`)，除非你还使用`--force`参数。

- **`-pn PROJECT_NAME, --project-name PROJECT_NAME`**

  项目的名称。您可以在项目的`project.json`文件中找到该名称。

- **`-o OUTPUT_PATH, --output-path OUTPUT_PATH`**

  写入`project_path`键的路径和文件名。该文件应位于`<USER_DIRECTORY>/.o3de/Registry/`中，文件扩展名为 `.setreg`。如果您不使用此参数，`o3de`会将注册表项写入`<USER_DIRECTORY>/.o3de/Registry/bootstrap.setreg`。

- **`-f, --force`**

  在提供的`.setreg`文件中强制设置项目路径。


<!-------------------------------------------------------------->

## `create-template`

从指定的源路径创建模板，并在 O3DE 清单中注册模板。然后，用户可以使用 `create-from-template` 命令从此模板创建自定义源内容。

### 格式

``` cmd
create-template [-h] [-v] -sp SOURCE_PATH [-tp TEMPLATE_PATH]
                [-srp SOURCE_RESTRICTED_PATH | -srn SOURCE_RESTRICTED_NAME]
                [-trp TEMPLATE_RESTRICTED_PATH | -trn TEMPLATE_RESTRICTED_NAME]
                [-sn SOURCE_NAME]
                [-srprp SOURCE_RESTRICTED_PLATFORM_RELATIVE_PATH]
                [-trprp TEMPLATE_RESTRICTED_PLATFORM_RELATIVE_PATH]
                [-kr] [-kl] [-r [REPLACE [REPLACE ...]]]
                [-f] [--no-register]
```

### 用法

从源文件夹创建名为`StandardGem`的模板，并将其保存在 O3DE 清单中指定的`default_templates_folder` 中。此外，在 O3DE 清单中注册模板。

```cmd
o3de.bat create-template -sp C:\MyGems\StandardGem
```

从源文件夹创建名为 `StandardGem` 的模板，并将其保存在 `C:\MyTemplates` 中，而不注册模板。

```cmd
o3de.bat create-template -sp C:\MyGems\StandardGem -tp C:\MyTemplates\StandardGem --no-register
```

从源文件夹创建名为`StandardGem`的模板，并将其保存在默认 templates 文件夹中。此外，它还会将源文件夹中包含单词`Standard`的文件或路径的任何部分替换为占位符名称`${Name}`，并将源文件内容中出现的单词`Standard`替换为 `${SanitizedCppName}`。

```cmd
o3de.bat create-template -sp C:\MyGems\TestGem -tp StandardGem -sn Standard
```

从源文件夹创建名为 `StandardComponent` 的模板，并将其保存在默认 templates 文件夹中。此外，它还执行以下操作：

* 将源文件夹中包含单词 `Standard` 的文件或路径的任何部分替换为占位符名称`${Name}`。
* 将源文件内容中出现的单词 `Standard` 替换为`${SanitizedCppName}`。
* 将单词`MyGem`的任何外观替换为`${GemName}`。在创建将 Gem 名称用作 C++ 命名空间的模板时，此特定示例非常有用。
* 将 UUID `cd2c4950-7ee3-49b9-b356-51a3b6bb2373` 替换为 `${Random_Uuid}`。当 `create-from-template` 命令在模板中遇到 `${Random_Uuid}` 时，它会将此占位符替换为随机生成的 UUID。
* 保留源文件中以 `{BEGIN_LICENSE}` 开头、以 `{END_LICENSE}` 结尾的所有许可文本。

```cmd
o3de.bat create-template -sp C:\MyComponent -tp StandardComponent -sn Standard -kl -r MyGem ${GemName} cd2c4950-7ee3-49b9-b356-51a3b6bb2373 ${Random_Uuid}
```

{{< note >}}
在 Windows PowerShell 中使用 replace 参数时，必须在任何 `$` 替换变量周围使用单引号。例如：`-r MyGem '${GemName}'`。
{{< /note >}}

### 可选参数
  
- **`-h, --help`**

  显示帮助消息。

- **`-v`**

  如果指定，则提供额外的日志记录详细程度。

- **`-sp SOURCE_PATH, --source-path SOURCE_PATH`**

  要用作模板的源文件夹的路径。

- **`-tp TEMPLATE_PATH, --template-path TEMPLATE_PATH`**

  要在其中创建模板的路径。该路径可以是绝对路径，也可以是相对于 O3DE 清单中指定的`default_templates_folder`路径。如果未提供，则模板路径将设置为与 `SOURCE_NAME` 连接的默认模板路径。如果未指定 `SOURCE_NAME`，则使用`SOURCE_PATH`的最后一个组件代替它。

- **`-srp SOURCE_RESTRICTED_PATH, --source-restricted-path SOURCE_RESTRICTED_PATH`**

  源的 restricted 文件夹的路径。

- **`-srn SOURCE_RESTRICTED_NAME, --source-restricted-name SOURCE_RESTRICTED_NAME`**

  源的 restricted 文件夹的名称。如果提供，则无需使用`--source-restricted-path`参数。

- **`-trp TEMPLATE_RESTRICTED_PATH, --template-restricted-path TEMPLATE_RESTRICTED_PATH`**

  模板的 restricted 文件夹的路径。

- **`-trn TEMPLATE_RESTRICTED_NAME, --template-restricted-name TEMPLATE_RESTRICTED_NAME`**

  模板的 restricted 文件夹的名称。如果提供，则无需使用 `--template-restricted-path` 参数。

- **`-sn SOURCE_NAME, --source-name SOURCE_NAME`**

  替换任何文件和路径条目，匹配 `SOURCE_NAME` 为 `${Name}`, 文件内容匹配 `SOURCE_NAME` 为 `${SanitizedCppName}`.
  
  替换示例：`--source-name Foo`替换源文件 `FooBus.h` -> `${Name}Bus.h`,和源内容 `class FooRequests` -> `class ${SanitizedCppName}Requests`.

- **`-srprp SOURCE_RESTRICTED_PLATFORM_RELATIVE_PATH, --source-restricted-platform-relative-path SOURCE_RESTRICTED_PLATFORM_RELATIVE_PATH`**

  路径附加到 `SOURCE_RESTRICTED_PATH/<platform>/`，其中包含受限源。例如： `--source-restricted-path C:/restricted --source-restricted-platform-relative-path some/folder` => `C:/restricted/<platform>/some/folder/<source_name>`.

- **`-trprp TEMPLATE_RESTRICTED_PLATFORM_RELATIVE_PATH, --template-restricted-platform-relative-path TEMPLATE_RESTRICTED_PLATFORM_RELATIVE_PATH`**

  路径附加到 `TEMPLATE_RESTRICTED_PATH/<platform>`，其中包含受限模板源。例如： `--template-restricted-path C:/restricted --template-restricted-platform-relative-path some/folder` => `C:/restricted/<platform>/some/folder/<template_name>`.

- **`-kr, --keep-restricted-in-template`**

  如果包含，则在模板文件夹中创建受限平台。默认情况下，在位于 TEMPLATE_RESTRICTED_PATH 的 restricted 文件夹中创建受限制的文件。

- **`-kl, --keep-license-text`**

  如果包含，则保留许可证文本的所有行，从 {BEGIN_LICENSE} 开始，到 {END_LICENSE} 结束。默认情况下，不包含许可证文本。

- **`-r [REPLACE [REPLACE ...]], --replace [REPLACE [REPLACE ...]]`**

  将 A 添加到 B 替换对。例如： `-r MyUsername ${User} 1723905 ${id}`。这将替换 `MyUsername` 为 `${User}`, 替换 `1723905` 为 `${id}`.

  {{< note >}}
在 Windows PowerShell 中使用 replace 参数时，必须在任何 `$` 替换变量周围使用单引号。例如： `-r MyUsername '${User}'`.
  {{< /note >}}

- **`-f, --force`**

  强制新模板目录覆盖现有模板目录（如果存在）。

- **`--no-register`**

  阻止在 O3DE 清单中注册模板路径。


<!-------------------------------------------------------------->

## `create-from-template`

基于指定的模板创建类属模板的实例。

### 格式

```cmd
create-from-template [-h] [-v] -dp DESTINATION_PATH
                                    (-tp TEMPLATE_PATH | -tn TEMPLATE_NAME)
                                    [-dn DESTINATION_NAME]
                                    [-drp DESTINATION_RESTRICTED_PATH | -drn DESTINATION_RESTRICTED_NAME]
                                    [-trp TEMPLATE_RESTRICTED_PATH | -trn TEMPLATE_RESTRICTED_NAME]
                                    [-drprp DESTINATION_RESTRICTED_PLATFORM_RELATIVE_PATH]
                                    [-trprp TEMPLATE_RESTRICTED_PLATFORM_RELATIVE_PATH]
                                    [-kr] [-kl] [-r [REPLACE [REPLACE ...]]]
                                    [-f] [--no-register]
```

### 用法

基于位于指定模板路径中的模板实例化对象，并将其保存在指定的目标路径中。

```cmd
o3de.bat create-from-template -dp DESTINATION_PATH -tp TEMPLATE_PATH
```

基于名为`DefaultComponent`的模板实例化一个对象，并将其保存在当前路径的`NewComponent`目录中。此外，它还将模板中出现的所有 `${GemName}` 替换为 `MyGem`。

```cmd
o3de.bat create-from-template -dp NewComponent -dn MyTest -tn DefaultComponent -kr -r ${GemName} MyGem
```

{{< note >}}
在 Windows PowerShell 中使用 replace 参数时，必须在任何 `$` 替换变量周围使用单引号。例： `-r '${GemName}' MyGem`.
{{< /note >}}

要在现有目录中创建组件，例如正在进行的 Gem 的 `Code` 目录，请在`create-from-template`命令中添加 `-f` 选项，以强制在该目录中创建组件文件。

例如，要在 Gem 的 `Code` 目录的`MyGem` 命名空间中创建名为 `MyTestComponent`的组件，请执行以下操作：

```cmd
scripts\o3de.bat create-from-template -dp C:\Gems\MyGem\Code -dn MyTest -tn DefaultComponent -kr -r ${GemName} MyGem -f
```

### 可选参数

- **`-h, --help`**

  显示帮助消息。

- **`-v`**

  如果指定，则提供额外的日志记录详细程度。

- **`-dp DESTINATION_PATH, --destination-path DESTINATION_PATH`**

  实例化新模板的路径。该路径可以是绝对路径，也可以是相对于 O3DE 开发源文件夹的路径。例如：DESTINATION_PATH = `C:/o3de/NewTemplate`.

- **`-tp TEMPLATE_PATH, --template-path TEMPLATE_PATH`**

  要实例化的模板的路径。该路径可以是绝对路径，也可以是相对于 O3DE 开发源文件夹的路径。例如： TEMPLATE_PATH = `C:/o3de/Template/SomeTemplate`.

- **`-tn TEMPLATE_NAME, --template-name TEMPLATE_NAME`**

  要实例化的已注册模板的名称。如果提供，则无需使用`--template-path` 参数。

- **`-dn DESTINATION_NAME, --destination-name DESTINATION_NAME`**

  在实例化模板中替换 `${Name}` 的名称。名称必须是字母数字，但可以包含下划线 （_） 和连字符 （-）。如果您未提供目标名称，则此命令将使用目标路径的最后一个组件。

- **`-drp DESTINATION_RESTRICTED_PATH, --destination-restricted-path DESTINATION_RESTRICTED_PATH`**

  `o3de` 写入受限制文件的路径。

- **`-drn DESTINATION_RESTRICTED_NAME, --destination-restricted-name DESTINATION_RESTRICTED_NAME`**

  `o3de` 写入受限制文件的已注册受限路径的名称。如果提供，则无需使用`--destination-restricted-path`参数。

- **`-trp TEMPLATE_RESTRICTED_PATH, --template-restricted-path TEMPLATE_RESTRICTED_PATH`**

  `o3de` 将受限平台源添加到的受限目录的路径（如果有）。

- **`-trn TEMPLATE_RESTRICTED_NAME, --template-restricted-name TEMPLATE_RESTRICTED_NAME`**

  `o3de` 将受限平台源添加到的受限目录的名称（如果有）。 如果提供，则无需使用`--template-restricted-path` 参数。

- **`-drprp DESTINATION_RESTRICTED_PLATFORM_RELATIVE_PATH, --destination-restricted-platform-relative-path DESTINATION_RESTRICTED_PLATFORM_RELATIVE_PATH`**

  要追加到`DESTINATION_RESTRICTED_PATH/<platform>`的路径 ，其中包含受限目标。例如： `--destination-restricted-path C:/instance --destination-restricted-platform-relative-path some/folder` => `C:/instance/<platform>/some/folder/<destination_name>`.

- **`-trprp TEMPLATE_RESTRICTED_PLATFORM_RELATIVE_PATH, --template-restricted-platform-relative-path TEMPLATE_RESTRICTED_PLATFORM_RELATIVE_PATH`**

  要追加到`TEMPLATE_RESTRICTED_PATH/<platform>`的路径 ，其中包含受限模板。例如： `--template-restricted-path C:/restricted --template-restricted-platform-relaive-path some/folder` => `C:/restricted/<platform>/some/folder/<template_name>`.

- **`-kr, --keep-restricted-in-instance`**

  如果指定，则在新模板文件夹中创建受限平台。如果未指定，则在位于 TEMPLATE_RESTRICTED_PATH 的 restricted 文件夹中创建受限制的文件。

- **`-kl, --keep-license-text`**

  如果指定，则保留在模板文件中找到的所有许可证文本。如果未指定，则不包括许可证文本。许可证文本包括从包含 {BEGIN_LICENSE} 的行开始到包含 {END_LICENSE} 的行结束的所有文本行。

- **`-r [REPLACE [REPLACE ...]], --replace [REPLACE [REPLACE ...]]`**

  将 A 添加到 B 替换对。例如： `-r ${User} MyUsername ${id} 1723905`. 这将替换 `${User}` 为 `MyUsername`, 替换 `${id}` 为 `1723905`. 注意: \<DestinationName\> 是DESTINATION_PATH的最后部分。以下替换对已存在：`${Name}` 替换为 \<DestinationName\>, `${NameLower}` 替换为 \<destinationname\>, `${NameUpper}` 替换为 \<DESTINATIONNAME\>.

  {{< note >}}
在 Windows PowerShell 中使用 replace 参数时，必须在任何 `$` 替换变量周围使用单引号。例如：`-r '${User}' MyUsername`.
  {{< /note >}}

- **`-f, --force`**

  覆盖目标目录中的文件（如果已存在）。

- **`--no-register`**

  阻止在 O3DE 清单中注册项目。


<!-------------------------------------------------------------->

## `create-project`

在指定路径创建新项目并将其注册到`o3de_manifest.json`文件中。但是，如果您在 engine 目录中创建项目，则此命令会将项目注册到引擎的`engine.json`文件中。

### 格式

``` cmd
create-project [-h] -pp PROJECT_PATH [-pn PROJECT_NAME]
                    [-tp TEMPLATE_PATH | -tn TEMPLATE_NAME]
                    [-prp PROJECT_RESTRICTED_PATH | -prn PROJECT_RESTRICTED_NAME]
                    [-trp TEMPLATE_RESTRICTED_PATH | -trn TEMPLATE_RESTRICTED_NAME]
                    [-prprp PROJECT_RESTRICTED_PLATFORM_RELATIVE_PATH]
                    [-trprp TEMPLATE_RESTRICTED_PLATFORM_RELATIVE_PATH]
                    [-kr] [-kl] [-r [REPLACE [REPLACE ...]]]
                    [--system-component-class-id SYSTEM_COMPONENT_CLASS_ID]
                    [--editor-system-component-class-id EDITOR_SYSTEM_COMPONENT_CLASS_ID]
                    [--module-id MODULE_ID] [-f]
```

### 用法

使用 “`DefaultProject`” 模板在指定路径处创建新项目。

```cmd
o3de.bat create-project -pp PROJECT_PATH
```

使用指定的项目模板在指定的路径处创建新项目。模板必须具有有效的 '`project.json`' 文件。

```cmd
o3de.bat create-project --project-path PROJECT_PATH --template-path TEMPLATE_PATH
```

### 可选参数

  
- **`-h, --help`**

  显示帮助消息。

- **`-pp PROJECT_PATH, --project-path PROJECT_PATH`**

  新创建的项目的路径。项目的路径可以是绝对路径，也可以是相对于 O3DE 开发源文件夹的路径。例如：PROJECT_PATH = `C:/o3de/TestProject`.

- **`-pn PROJECT_NAME, --project-name PROJECT_NAME`**

  新项目的名称。名称必须是字母数字，但可以包含下划线 （_） 和连字符 （-）。如果您未提供项目名称，则此命令将使用项目路径的最后一个组件。例如：New_Project-123。

- **`-tp TEMPLATE_PATH, --template-path TEMPLATE_PATH`**

  要从中创建新项目的模板的路径。该路径可以是绝对路径，也可以是相对于默认模板路径的路径。

- **`-tn TEMPLATE_NAME, --template-name TEMPLATE_NAME`**

  要从中创建新项目的模板的名称。如果提供，则无需使用 `--template-path`参数。

- **`-prp PROJECT_RESTRICTED_PATH, --project-restricted-path PROJECT_RESTRICTED_PATH`**

  项目的 restricted 文件夹的路径。路径可以是绝对路径，也可以是相对于`restricted="projects"`的路径。

- **`-prn PROJECT_RESTRICTED_NAME, --project-restricted-name PROJECT_RESTRICTED_NAME`**

  项目的受限路径的名称。如果提供，则无需使用`--project-restricted-path`参数。

- **`-trp TEMPLATE_RESTRICTED_PATH, --template-restricted-path TEMPLATE_RESTRICTED_PATH`**

  模板的 restricted 文件夹的路径。路径可以是绝对路径，也可以是相对于`restricted="templates"`的路径。

- **`-trn TEMPLATE_RESTRICTED_NAME, --template-restricted-name TEMPLATE_RESTRICTED_NAME`**

  模板的受限路径的名称。如果提供，则无需使用`--template-restricted-path`参数。

- **`-prprp PROJECT_RESTRICTED_PLATFORM_RELATIVE_PATH, --project-restricted-platform-relative-path PROJECT_RESTRICTED_PLATFORM_RELATIVE_PATH`**

  附加到`PROJECT_RESTRICTED_PATH/<platform>`的路径，其中包含受限项目。例如：`--project-restricted-path C:/restricted --project- restricted-platform-relative-path some/folder` => `C:/restricted/<platform>/some/folder/<project_name>`.

- **`-trprp TEMPLATE_RESTRICTED_PLATFORM_RELATIVE_PATH, --template-restricted-platform-relative-path TEMPLATE_RESTRICTED_PLATFORM_RELATIVE_PATH`**

  附加到`TEMPLATE_RESTRICTED_PATH/<platform>`，其中包含受限模板源。例如：`--template-restricted-path C:/restricted --template-restricted-platform-relative-path some/folder` => `C:/restricted/<platform>/some/folder/<template_name>`.

- **`-kr, --keep-restricted-in-project`**

  如果为 true，则在项目文件夹中创建受限平台。如果为 false，则在位于 TEMPLATE_RESTRICTED_PATH 的受限文件夹中创建受限文件。默认情况下，此参数为 false。

- **`-kl, --keep-license-text`**

  如果为 true，则将许可证文本（位于`template.json`文件中）保留在新项目的`project.json`文件中。如果为 false，则不包含许可证文本。默认情况下，此参数为 false。许可证文本是所有文本行，从 {BEGIN_LICENSE} 开始，到 {END_LICENSE} 结束。

- **`-r [REPLACE [REPLACE ...]], --replace [REPLACE [REPLACE ...]]`**

  将 A 添加到 B 替换对。`o3de`会自动从项目名称推断出 `${Name}` 和所有其他标准项目替换。这些替换将取代所有推断的替换。例如：`--replace MyUsername ${User} 1723905 ${id}`。这会将`MyUsername`替换为`${User}`，将`1723905`替换为`${id}`。注意：\<ProjectName\> 是template_path的最后一个组件。以下替换对已存在：`${Name}` 到 \<ProjectName\>，`${NameLower}` 到 \<projectname\>，'`${NameUpper}` 到 \<PROJECTNAME\>。

- **`--system-component-class-id SYSTEM_COMPONENT_CLASS_ID`**

  要与系统类组件关联的 UUID。默认值为随机 UUID。例如
{b60c92eb-3139-454b-a917-a9d3c5819594}.

- **`--editor-system-component-class-id EDITOR_SYSTEM_COMPONENT_CLASS_ID`**

  要与 Editor 系统类组件关联的 UUID。默认值为随机 UUID。例如{b60c92eb-3139-454b-a917-a9d3c5819594}.

- **`--module-id MODULE_ID`**

  要与模块关联的 UUID。默认值为随机 UUID。例如 {b60c92eb-3139-454b-a917-a9d3c5819594}. 

- **`-f, --force`**

  强制新项目目录覆盖现有项目目录（如果存在）。


<!-------------------------------------------------------------->

## `create-gem`

在指定路径处创建新 Gem。

### 格式

```cmd
create-gem [-h] -gp GEM_PATH [-gn GEM_NAME]
                    [-tp TEMPLATE_PATH | -tn TEMPLATE_NAME]
                    [-grp GEM_RESTRICTED_PATH | -grn GEM_RESTRICTED_NAME]
                    [-trp TEMPLATE_RESTRICTED_PATH | -trn TEMPLATE_RESTRICTED_NAME]
                    [-grprp GEM_RESTRICTED_PLATFORM_RELATIVE_PATH]
                    [-trprp TEMPLATE_RESTRICTED_PLATFORM_RELATIVE_PATH]
                    [-r [REPLACE [REPLACE ...]]] [-kr] [-kl]
                    [--system-component-class-id SYSTEM_COMPONENT_CLASS_ID]
                    [--editor-system-component-class-id EDITOR_SYSTEM_COMPONENT_CLASS_ID]
                    [--module-id MODULE_ID] [-f]
```

### 用法

使用 “DefaultGem” 模板在指定路径处创建一个新 Gem。

```cmd
o3de.bat create-gem -gp GEM_PATH
```

使用指定的 Gem 模板在指定的路径处创建新 Gem。模板必须具有有效的`gem.json`文件。

```cmd
o3de.bat create-gem -gp GEM_PATH --template-path TEMPLATE_PATH
```

### 可选参数

- **`-h, --help`**

  显示帮助消息。

- **`-gp GEM_PATH, --gem-path GEM_PATH`**

  创建新 Gem 的路径。Gem 的路径可以是绝对路径，也可以是相对于默认 Gem 路径的路径。  

- **`-gn GEM_NAME, --gem-name GEM_NAME`**

  新 Gem 的名称。名称必须是字母数字，但可以包含下划线 （_） 和连字符 （-）。如果您未提供 Gem 名称，则此命令将使用 Gem 路径的最后一个组件。例如：New_Gem。

- **`-tp TEMPLATE_PATH, --template-path TEMPLATE_PATH`**

  要从中创建新 Gem 的模板的路径。该路径可以是绝对路径，也可以是相对于默认模板路径的路径。

- **`-tn TEMPLATE_NAME, --template-name TEMPLATE_NAME`**

  要从中创建新 Gem 的模板的名称。如果提供，则无需使用`--template-path`参数。

- **`-grp GEM_RESTRICTED_PATH, --gem-restricted-path GEM_RESTRICTED_PATH`**

  Gem 的 restricted 文件夹的路径（如果有）。该路径可以是绝对路径，也可以是相对于 O3DE 开发源文件夹的路径。默认值为`<o3de-development-source>/restricted`.

- **`-grn GEM_RESTRICTED_NAME, --gem-restricted-name GEM_RESTRICTED_NAME`**

  Gem 的受限路径的名称 （如果有）。如果提供，则无需使用`--gem-restricted-path` 参数。

- **`-trp TEMPLATE_RESTRICTED_PATH, --template-restricted-path TEMPLATE_RESTRICTED_PATH`**

  模板的 restricted 文件夹的路径。路径可以是绝对路径，也可以是相对于`restricted="templates"`的路径。

- **`-trn TEMPLATE_RESTRICTED_NAME, --template-restricted-name TEMPLATE_RESTRICTED_NAME`**

  模板的受限路径的名称。如果提供，则无需使用`--template-restricted-path`参数。

- **`-grprp GEM_RESTRICTED_PLATFORM_RELATIVE_PATH, --gem-restricted-platform-relative-path GEM_RESTRICTED_PLATFORM_RELATIVE_PATH`**

  附加到`GEM_RESTRICTED_PATH/<platform>`的路径，其中包含受限模板。例如： `--gem-restricted-path C:/restricted --gem-restricted- platform-relative-path some/folder` => `C:/restricted/<platform>/some/folder/<gem_name>`.

- **`-trprp TEMPLATE_RESTRICTED_PLATFORM_RELATIVE_PATH, --template-restricted-platform-relative-path TEMPLATE_RESTRICTED_PLATFORM_RELATIVE_PATH`**

  附加到`TEMPLATE_RESTRICTED_PATH/<platform>`的路径，其中包含受限模板。例如： `--template-restricted-path C:/restricted --template-restricted-platform-relaive-path some/folder` => `C:/restricted/<platform>/some/folder/<template_name>`.

- **`-r [REPLACE [REPLACE ...]], --replace [REPLACE [REPLACE ...]]`**

  将 A 添加到 B 替换对。`o3de`会自动从 Gem 名称推断出 `${Name}` 和所有其他标准 Gem 替换。这些替换将取代所有推断的替换对。例如：`--replace ${DATE} 1/1/2020 ${id} 1723905`。这会将`${DATE}`替换为 `1/1/2020`，将 `${id}` 替换为 `1723905`。注： \<GemName> 是 GEM_PATH 的最后一个组成部分。以下替换对已存在：`${Name}`到 \<GemName\>，`${NameLower}`到 \<gemname\>，`${NameUpper}` 到 \<GEMNAME\>。

- **`-kr, --keep-restricted-in-gem`**

  如果为 true，则在 Gem 文件夹中创建受限平台。如果为 false，则在位于 TEMPLATE_RESTRICTED_PATH 的受限文件夹中创建受限文件。默认情况下，此参数为 false。

- **`-kl, --keep-license-text`**

  如果为 true，则将许可证文本（位于`template.json`文件中）保留在新 Gem 的`gem.json`文件中。如果为 false，则不包含许可证文本。默认情况下，此参数为 false。许可证文本是所有文本行，从 {BEGIN_LICENSE} 开始，到 {END_LICENSE} 结束。

- **`--system-component-class-id SYSTEM_COMPONENT_CLASS_ID`**

  要与系统类组件关联的 UUID。默认值为随机 UUID。例如：{b60c92eb-3139-454b-a917-a9d3c5819594}.

- **`--editor-system-component-class-id EDITOR_SYSTEM_COMPONENT_CLASS_ID`**

  要与 Editor 系统类组件关联的 UUID。默认值为随机 UUID。例如：{b60c92eb-3139-454b-a917-a9d3c5819594}.

- **`--module-id MODULE_ID`**

  要与模块关联的 UUID。默认值为随机 UUID。例如： {b60c92eb-3139-454b-a917-a9d3c5819594}.

- **`-f, --force`**

  强制新的 Gem 目录覆盖现有 Gem 目录（如果存在）。


<!-------------------------------------------------------------->

## `register`

将 O3DE 对象注册到`o3de_manifest.json`文件。

### 格式

```cmd
register [-h]
                        (--this-engine | -ep ENGINE_PATH | -pp PROJECT_PATH | -gp GEM_PATH | -es EXTERNAL_SUBDIRECTORY | -tp TEMPLATE_PATH | -rp RESTRICTED_PATH | -ru REPO_URI | -aep ALL_ENGINES_PATH | -app ALL_PROJECTS_PATH | -agp ALL_GEMS_PATH | -atp ALL_TEMPLATES_PATH | -arp ALL_RESTRICTED_PATH | -aru ALL_REPO_URI | -def DEFAULT_ENGINES_FOLDER | -dpf DEFAULT_PROJECTS_FOLDER | -dgf DEFAULT_GEMS_FOLDER | -dtf DEFAULT_TEMPLATES_FOLDER | -drf DEFAULT_RESTRICTED_FOLDER | -dtpf DEFAULT_THIRD_PARTY_FOLDER | -u)
                        [-r] [-f | -dry]
                        [-esep EXTERNAL_SUBDIRECTORY_ENGINE_PATH | -espp EXTERNAL_SUBDIRECTORY_PROJECT_PATH | -esgp EXTERNAL_SUBDIRECTORY_GEM_PATH | -frwom]
```


### 用法
<br>

**注册引擎**

将此引擎注册到`o3de_manifest.json`文件中。 此命令将引擎的名称映射到其在 JSON 中的路径。

```cmd
o3de.bat register --this-engine
```

将指定的引擎注册到`o3de_manifest.json`文件。 此命令将引擎的名称映射到其在 JSON 中的路径。  

```cmd
o3de.bat register --engine-path ENGINE_PATH
```

将指定路径中的所有引擎注册到`o3de_manifest.json`文件。此命令以递归方式扫描指定的路径，并注册具有有效`engine.json`文件的所有路径。它将每个引擎的名称映射到其在 JSON 中的路径。

```cmd
o3de.bat register --all-engines-path ALL_ENGINES_PATH
```

<br>

**注册项目**

将指定的项目注册到引擎。此命令执行两项操作：将项目的路径添加到`o3de_manifest.json`文件，并将 `engine`设置为每个项目的`project.json`文件中的引擎名称。

```cmd
o3de.bat register -pp PROJECT_PATH
```

将指定的项目注册到指定的引擎。此命令执行两项操作：将项目的路径添加到`o3de_manifest.json`文件，并将`engine`设置为每个项目的`project.json`文件中的引擎名称。

```cmd
o3de.bat register -pp PROJECT_PATH --engine-path ENGINE_PATH
```

在指定路径中注册所有项目。此命令以递归方式扫描指定的路径，并注册具有有效`project.json`文件的所有路径。这个命令做两件事：将每个项目的路径添加到 `o3de_manifest.json` 文件，并将 `engine` 设置为每个项目的`project.json`文件中的引擎名称。

```cmd
o3de.bat register --all-projects-path ALL_PROJECTS_PATH
```

<br>

**注册Gem**

将 Gem 注册到 Gem 所在位置上方最近的 o3de 对象清单文件。在注册 Gem 之前，此命令将验证 Gem 是否具有有效的`gem.json`文件。然后，它将 Gem 的路径添加到 Gem 位置上方最近的 o3de 对象清单文件中的`external_subdirectories`列表中。

```cmd
o3de.bat register --gem-path GEM_PATH
```

例如，给定以下使用 o3de 的文件系统布局，将注册 Gem 的清单文件会根据注册 Gem 的位置而变化。
```
/home/user/.o3de
    /o3de_manifest.json
/home/user/repo1
    /o3de <-- engine source code
    /project  <-- project
    /anothergem <-- A random gem
```

* 如果 `GemToRegister` 是在 `anothergem` 中，位于 `/home/user/repo1/anothergem/GemToRegister`，然后它将被注册在`/home/user/repo1/anothergem/gem.json` manifest中。
* 如果 `GemToRegister` 是在项目的  `Gems` 文件夹中，位于 `/home/user/repo1/project/Gems/GemToRegister`，然后它将被注册在`/home/user/repo1/project/project.json` manifest中。
* 如果 `GemToRegister` 是在引擎的 `Gems` 文件夹中，位于 `/home/user/repo1/o3de/Gems/GemToRegister`，然后它将被注册在`/home/user/repo1/o3de/engine.json` manifest中。
* 最终，如果 `GemToRegister` 在引擎、项目或其它Gem目录之外，然后它将被注册在`o3de_manifest.json`中，例如`/home/user/MyGems/GemToRegister` 将在`/home/user/.o3de/o3de_manifest.json`中注册Gem



将 Gem 注册到项目的`project.json`文件中。在注册 Gem 之前，此命令将验证 Gem 是否具有有效的`gem.json`文件。然后，它将 Gem 的路径添加到`project.json`文件中的`external_subdirectories`列表中。这会将 Gem 添加到项目的构建解决方案中，以便项目可以识别 Gem。

```cmd
o3de.bat register -gp GEM_PATH -espp PROJECT_PATH
```

将 Gem 注册到引擎的 `engine.json` 文件中。在注册 Gem 之前，此命令将验证 Gem 是否具有有效的 `gem.json` 文件。然后，它将 Gem 的路径添加到`engine.json`文件中的`external_subdirectories`列表中。这会将 Gem 添加到引擎的构建解决方案中，以便引擎可以识别 Gem。

```cmd
o3de.bat register -gp GEM_PATH -espp ENGINE_PATH
```

注册指定路径中的所有 Gem。此命令以递归方式扫描指定的路径，并注册具有有效`gem.json`文件的所有路径。此命令将每个 Gem 添加到 `o3de_manifest.json` 文件中。

```cmd
o3de.bat register --all-gems-path ALL_GEMS_PATH
```

<br>

**注册外部子目录**

将指定的子目录路径注册到`o3de_manifest.json`文件中的 `external_subdirectories` 列表。

```cmd
o3de.bat register --external-subdirectory EXTERNAL_SUBDIRECTORY
```

将指定的子目录路径注册到指定引擎的 `engine.json` 文件的 `external_subdirectories` 中。

```cmd
o3de.bat register --external-subdirectory EXTERNAL_SUBDIRECTORY --external-subdirectory-engine-path ENGINE_PATH
```

将指定的子目录路径注册到指定项目的 `project.json` 文件的 `external_subdirectories` 中。

```cmd
o3de.bat register --external-subdirectory EXTERNAL_SUBDIRECTORY --external-subdirectory-project-path PROJECT_PATH
```

<br>

**注册模板**

将指定的模板注册到 `o3de_manifest.json` 文件的 `templates` 列表中。

```cmd
o3de.bat register --template-path <template-path>
```

将指定的模板注册到指定引擎的 `templates` 文件的`engine.json`列表中。

```cmd
o3de.bat register --template-path TEMPLATE_PATH --engine-path ENGINE_PATH
```

在指定路径中注册所有模板。此命令以递归方式扫描指定的路径，并注册具有有效`template.json`文件的所有路径。此命令将每个模板添加到`o3de_manifest.json`文件的`templates` 列表中。

```cmd
o3de.bat register --all-templates-path ALL_TEMPLATES_PATH
```

<br>

**注册受限制的路径**

将指定的受限路径注册到`o3de_manifest.json`文件中的`restricted`列表。

```cmd
o3de.bat register --restricted-path RESTRICTED_PATH
```

将指定路径中的所有受限路径注册到`o3de_manifest.json`文件中的 `restricted` 列表。

```cmd
o3de.bat register --all-restricted-path ALL_RESTRICTED_PATH
```

<br>

**注册存储库**

将指定的存储库统一资源标识符 （URI） 注册到`o3de_manifest.json`文件中的`repos`列表中。

```cmd
o3de.bat register --repo-uri REPO_URI
```

在指定的 URI 中注册所有存储库。此命令以递归方式扫描指定的路径，并注册具有有效`repo.json`文件的所有存储库。此命令将每个存储库添加到`o3de_manifest.json`文件中。

```cmd
o3de.bat register --all-repo-uri ALL_REPO_URI
```

<br>

**注册默认文件夹**

将指定的文件夹注册为 `o3de_manifest.json` 文件中的默认 engines 目录。

```cmd
o3de.bat register --default-engines-folder ENGINES_FOLDER_PATH
```

将指定的文件夹注册为`o3de_manifest.json`文件中的默认项目目录。使用`create-project`命令创建项目时，如果提供相对路径，则项目将保存在默认项目文件夹中。

```cmd
o3de.bat register --default-projects-folder PROJECTS_FOLDER_PATH
```

将指定的文件夹注册为 `o3de_manifest.json`文件中的默认 Gems 目录。使用 'create-gem' 命令创建 Gem 时，如果提供相对路径，则 Gem 将保存在默认的 Gems 文件夹中。

```cmd
o3de.bat register --default-gems-folder GEMS_FOLDER_PATH
```

将指定的文件夹注册为`o3de_manifest.json`文件中的默认模板目录。使用`create-template`命令创建模板时，如果提供相对路径，模板将保存在默认模板文件夹中。

```cmd
o3de.bat register --default-templates-folder TEMPLATES_FOLDER_PATH
```

将指定的文件夹注册为`o3de_manifest.json` 文件中的默认受限目录。命令 --`create-from-template`, `create-project`, 和 `create-gem` --在实例化模板时使用默认的受限目录。

```cmd
o3de.bat register --default-restricted-folder RESTRICTED_FOLDER_PATH
```

将指定的文件夹注册为`o3de_manifest.json`文件中的默认第三方目录。**Project Manager** 在配置 CMake 构建解决方案时使用默认的第三方目录来设置 `LY_3RDPARTY_PATH` 选项。

```cmd
o3de.bat register --default-third-party folder THIRD_PARTY_FOLDER_PATH
```

<br>

**更新 `o3de_manifest.json` 文件**

更新 `o3de_manifest.json` 文件。此命令将验证所有已注册的对象，然后删除无效对象。

```cmd
o3de.bat register --update
```


### 可选参数

- **`-h, --help`**

  显示帮助消息。

- **`--this-engine`**

  运行此 O3DE Python 脚本的引擎。

- **`-ep ENGINE_PATH, --engine-path ENGINE_PATH`**

  要注册或取消注册的引擎的路径。

- **`-pp PROJECT_PATH, --project-path PROJECT_PATH`**

  要注册或取消注册的项目的路径。

- **`-gp GEM_PATH, --gem-path GEM_PATH`**

  要注册或取消注册的 Gem 的路径。

- **`-es EXTERNAL_SUBDIRECTORY, --external-subdirectory EXTERNAL_SUBDIRECTORY`**

  要注册或取消注册的外部子目录的路径。

- **`-tp TEMPLATE_PATH, --template-path TEMPLATE_PATH`**

  要注册或取消注册的模板的路径。

- **`-rp RESTRICTED_PATH, --restricted-path RESTRICTED_PATH`**

  要注册或取消注册的受限文件夹的路径。

- **`-ru REPO_URI, --repo-uri REPO_URI`**

  要注册或取消注册的存储库的路径。

- **`-aep ALL_ENGINES_PATH, --all-engines-path ALL_ENGINES_PATH`**

  要在指定文件夹中注册或取消注册的所有引擎。

- **`-app ALL_PROJECTS_PATH, --all-projects-path ALL_PROJECTS_PATH`**

  要在指定文件夹中注册或取消注册的所有项目。

- **`-agp ALL_GEMS_PATH, --all-gems-path ALL_GEMS_PATH`**

  要在指定文件夹中注册或取消注册的所有 Gem。

- **`-atp ALL_TEMPLATES_PATH, --all-templates-path ALL_TEMPLATES_PATH`**

  要在指定文件夹中注册或取消注册的所有模板。

- **`-arp ALL_RESTRICTED_PATH, --all-restricted-path ALL_RESTRICTED_PATH`**
  
  要在指定文件夹中注册或取消注册的所有受限文件夹。

- **`-aru ALL_REPO_URI, --all-repo-uri ALL_REPO_URI`**
  
  要在指定文件夹中注册或取消注册的所有存储库。

- **`-def DEFAULT_ENGINES_FOLDER, --default-engines-folder DEFAULT_ENGINES_FOLDER`**

  要注册或取消注册的默认 engines 文件夹的路径。

- **`-dpf DEFAULT_PROJECTS_FOLDER, --default-projects-folder DEFAULT_PROJECTS_FOLDER`**

  要注册或取消注册的默认项目文件夹的路径。

- **`-dgf DEFAULT_GEMS_FOLDER, --default-gems-folder DEFAULT_GEMS_FOLDER`**

  要注册或取消注册的默认 Gems 文件夹的路径。

- **`-dtf DEFAULT_TEMPLATES_FOLDER, --default-templates-folder DEFAULT_TEMPLATES_FOLDER`**

  要注册或取消注册的默认 templates 文件夹的路径。

- **`-drf DEFAULT_RESTRICTED_FOLDER, --default-restricted-folder DEFAULT_RESTRICTED_FOLDER`**

  要注册或取消注册的默认 restricted 文件夹的路径。

- **`-dtpf DEFAULT_THIRD_PARTY_FOLDER, --default-third-party-folder DEFAULT_THIRD_PARTY_FOLDER`**

  要注册或取消注册的默认第三方文件夹的路径。

- **`-u, --update`**

  刷新存储库缓存。

- **`-r, --remove`**

  取消注册指定的条目。

- **`-f, --force`**

  如果您进行了任何修改，则强制更新注册信息。

- **`-dry, --dry-run`**

  执行试运行，报告结果，但实际上不会更改任何内容。

 
**external-subdirectory:**  
将以下参数与`--external-subdirectory`选项一起使用。

- **`-esep EXTERNAL_SUBDIRECTORY_ENGINE_PATH, --external-subdirectory-engine-path EXTERNAL_SUBDIRECTORY_ENGINE_PATH`**

  如果提供，则在指定的引擎路径中用`engine.json`文件注册 external 子目录。

- **`-espp EXTERNAL_SUBDIRECTORY_PROJECT_PATH, --external-subdirectory-project-path EXTERNAL_SUBDIRECTORY_PROJECT_PATH`**

  如果提供，则在指定的项目路径中用 `project.json` 文件注册 external 子目录。

- **`-esgp EXTERNAL_SUBDIRECTORY_GEM_PATH, --external-subdirectory-gem-path EXTERNAL_SUBDIRECTORY_GEM_PATH`**

  如果提供，则在 gem-path 位置使用`gem.json`注册外部子目录。

- **`-frwom, --force-register-with-o3de-manifest`**

  设置后，强制使用`~/.o3de/o3de_manifest.json`注册外部子目录。

<!-------------------------------------------------------------->

## `register-show`

显示在`o3de_manifest.json` 文件中注册的 O3DE 对象。

### 格式

```cmd
register-show [-h]
                             [-te | -e | -p | -g | -t | -r | -rs | -ep | -eg | -et | -ers | -ees | -pg | -pt | -prs | -pes | -ap | -ag | -at | -ares | -aes]
                             [-v] [-pp PROJECT_PATH | -pn PROJECT_NAME]
                             [-ohf OVERRIDE_HOME_FOLDER]
```

### 用法

输出 `o3de_manifest.json` 文件。

```cmd
o3de.bat register-show
```

<br>

**显示已注册的引擎**

输出在`o3de_manifest.json` 文件中注册的当前引擎。如果使用 `-v`，此命令还会输出当前引擎的`engine.json`文件的内容。

```cmd
o3de.bat register-show --this-engine -v
```

输出在`o3de_manifest.json`文件中注册的引擎列表。如果使用 `-v`，则此命令将输出每个引擎的`engine.json`文件。

```cmd
o3de.bat register-show --engines -v
```

<br>

**显示已注册的项目**

输出在`o3de_manifest.json`文件中注册的项目列表。如果使用`-v`，则此命令将输出每个项目的`project.json`文件。

```cmd
o3de.bat register-show --projects -v
```

输出在当前引擎的`engine.json`文件中注册的项目列表。如果使用`-v`，则此命令将输出每个项目的 `project.json` 文件。

```cmd
o3de.bat register-show --engine-projects -v
```

输出在`o3de_manifest.json`文件和当前引擎的 `engine.json` 文件中注册的项目列表。如果使用`-v`，则此命令将输出每个项目的`project.json`文件。

```cmd
o3de.bat register-show --all-projects -v
```

<br>

**显示已注册的 Gem**

输出在`o3de_manifest.json`文件中注册的 Gem 列表。如果您使用 `-v`，则此命令将输出每个 Gem 的 `gem.json` 文件。

```cmd
o3de.bat register-show --gems -v
```

输出在当前引擎的`engine.json`文件中注册的 Gem 列表。如果您使用 `-v`，则此命令将输出每个 Gem 的 `gem.json` 文件。

```cmd
o3de.bat register-show --engine-gems -v
```

输出在指定项目的 `project.json` 文件中注册的 Gem 列表。如果您使用 `-v`，则此命令将输出每个 Gem 的 `gem.json` 文件。

```cmd
o3de.bat register-show --project-gems --project-path PROJECT_PATH -v
```

输出在 `o3de_manifest.json` 文件和当前引擎的`engine.json`文件中注册的 Gem 列表。如果您使用 `-v`，则此命令将输出每个 Gem 的 `gem.json` 文件。

```cmd
o3de.bat register-show --all-gems -v
```

<br>

**显示已注册的模板**

输出在`o3de_manifest.json`文件中注册的模板列表。如果使用 `-v`，则此命令将输出每个模板的`template.json`文件。

```cmd
o3de.bat register-show --templates -v
```

输出在当前引擎的`engine.json`文件中注册的模板列表。如果使用 `-v`，则此命令将输出每个模板的`template.json`文件。

```cmd
o3de.bat register-show --engine-templates -v
```

输出在指定项目的`project.json`文件中注册的模板列表。如果使用 `-v`，则此命令将输出每个模板的`template.json`文件。

```cmd
o3de.bat register-show --project-templates --project-path PROJECT_PATH -v
```

输出在 `o3de_manifest.json`  文件和当前引擎的 `engine.json` 文件中注册的模板列表。如果使用 `-v`，则此命令将输出每个模板的`template.json`文件。

```cmd
o3de.bat register-show --all-templates -v
```

<br>

**显示已注册的受限目录**

输出在`o3de_manifest.json`文件中注册的受限目录的列表。如果使用 `-v`，则此命令将输出每个受限目录的`restricted.json`文件。

```cmd
o3de.bat register-show --restricted -v
```

输出在当前引擎的 'engine.json' 文件中注册的受限目录的列表。 如果使用 `-v`，则此命令将输出每个受限目录的`restricted.json`文件。

```cmd
o3de.bat register-show --engine-restricted -v
```

输出在指定项目的 `project.json` 文件中注册的受限目录列表。如果使用 `-v`，则此命令将输出每个受限目录的`restricted.json`文件。

```cmd
o3de.bat register-show --project-restricted --project-path PROJECT_PATH -v
```

输出在`o3de_manifest.json`文件和当前引擎的 `engine.json` 文件中注册的受限目录列表。如果使用 `-v`，则此命令将输出每个受限目录的`restricted.json`文件。

```cmd
o3de.bat register-show --all-restricted -v
```

<br>

**显示已注册的外部子目录**

输出在 `o3de_manifest.json` 文件中注册的外部子目录的列表。

```cmd
o3de.bat register-show --external-subdirectories
```

输出在当前引擎的`engine.json`文件中注册的外部子目录的列表。

```cmd
o3de.bat register-show --engine-external-subdirectories
```

输出在指定项目的`project.json`文件中注册的外部子目录的列表。

```cmd
o3de.bat register-show --project-external-subdirectories --project-path PROJECT_PATH
```

输出在 `o3de_manifest.json`文件和当前引擎的 `engine.json` 文件中注册的外部子目录的列表。

```cmd
o3de.bat register-show --all-external-subdirectories
```

<br>

**显示已注册的存储库**

输出在 `o3de_manifest.json` 文件中注册的存储库列表。

```cmd
o3de.bat register-show --repos
```


### 可选参数

- **`-h, --help`**

  显示帮助消息。

- **`-te, --this-engine`**

  输出当前引擎的路径。

- **`-e, --engines`**

  输出在 `o3de_manifest.json` 文件中注册的引擎列表。

- **`-p, --projects`**

  输出在`o3de_manifest.json` 文件中注册的项目列表。

- **`-g, --gems`**

  输出在 `o3de_manifest.json` 文件中注册的 Gem 的列表。

- **`-t, --templates`**

  输出在 `o3de_manifest.json` 文件中注册的模板列表。

- **`-r, --repos`**

  输出在`o3de_manifest.json` 文件中注册的存储库列表。

- **`-rs, --restricted`**

  输出在 `o3de_manifest.json` 文件中注册的受限目录的列表。

- **`-ep, --engine-projects`**

  输出在当前引擎的 `engine.json` 文件中注册的项目列表。 

- **`-eg, --engine-gems`**

  输出在当前引擎的 `engine.json` 文件中注册的 Gem 列表。

- **`-et, --engine-templates`**

  输出在当前引擎的 `engine.json` 文件中注册的模板列表。

- **`-ers, --engine-restricted`**

  输出在当前引擎的 `engine.json` 文件中注册的受限目录的列表。

- **`-ees, --engine-external-subdirectories`**

  输出在当前引擎的`engine.json`文件中注册的外部子目录的列表。

- **`-pg, --project-gems`**

  输出在指定项目的`project.json`文件中注册的 Gem 列表。 您必须使用 `-pp` 或 `-pn` 选项指定项目。

- **`-pt, --project-templates`**

  输出在指定项目的 `project.json`文件中注册的模板列表。 您必须使用 `-pp` 或 `-pn` 选项指定项目。

- **`-prs, --project-restricted`**

  输出在指定项目的 `project.json` 文件中注册的受限目录的列表。您必须使用 `-pp` 或 `-pn` 选项指定项目。

- **`-pes, --project-external-subdirectories`**

  输出在指定项目的 `project.json` 文件中注册的外部子目录的列表。您必须使用 `-pp` 或 `-pn` 选项指定项目。

- **`-ap, --all-projects`**

  输出在 `o3de_manifest.json` 文件和当前引擎的 `engine.json` 文件中注册的所有项目。 

- **`-ag, --all-gems`**

  输出在`o3de_manifest.json`文件和当前引擎的`engine.json`文件中注册的所有 Gem。

- **`-at, --all-templates`**

  输出在`o3de_manifest.json`文件和当前引擎的`engine.json`文件中注册的所有模板。

- **`-ares, --all-restricted`**

  输出在`o3de_manifest.json`文件和当前引擎的`engine.json`文件中注册的所有受限目录。

- **`-aes, --all-external-subdirectories`**

  输出在`o3de_manifest.json`文件和当前引擎的`engine.json` 文件中注册的所有外部子目录。

- **`-v, --verbose`**

  如果指定，则输出所列文件的内容。

- **`-pp PROJECT_PATH, --project-path PROJECT_PATH`**

  项目的路径。

- **`-pn PROJECT_NAME, --project-name PROJECT_NAME`**

  项目的名称。

- **`-ohf OVERRIDE_HOME_FOLDER, --override-home-folder OVERRIDE_HOME_FOLDER`**

  使用指定的文件夹覆盖默认主文件夹。默认情况下，主文件夹是用户文件夹。



<!-------------------------------------------------------------->

## `get-registered`

显示具有指定名称的已注册 O3DE 对象的路径。

### 格式

```cmd
get-registered [-h]
                    (-en ENGINE_NAME | -pn PROJECT_NAME | -gn GEM_NAME | -tn TEMPLATE_NAME | -df {engines,projects,gems,templates,restricted} | -rn REPO_NAME | -rsn RESTRICTED_NAME)
                    [-ohf OVERRIDE_HOME_FOLDER]
```

### 用法

返回具有指定 `engine_name` 值的引擎的第一个路径。

```cmd
o3de.bat get-registered --engine-name ENGINE_NAME
```

返回具有指定`project_name`值的项目的第一个路径。

```cmd
o3de.bat get-registered --project-name PROJECT_NAME
```

返回具有指定 `gem_name` 值的 Gem 的第一个路径。

```cmd
o3de.bat get-registered --gem-name GEM_NAME
```

返回具有指定 `template_name` 值的模板的第一个路径。

```cmd
o3de.bat get-registered --template-name TEMPLATE_NAME
```

返回具有指定 `restricted_name` 值的受限目录的第一个路径。

```cmd
o3de.bat get-registered --restricted-name RESTRICTED_NAME
```

返回具有指定 `repo_name` 值的存储库的第一个 URI。

```cmd
o3de.bat get-registered --repo-name REPO_NAME
```

返回在 `o3de_manifest.json` 文件中注册的默认 engine 文件夹。

```cmd
o3de.bat get-registered --default-folder engines
```

返回在 `o3de_manifest.json` 文件中注册的默认项目文件夹。

```cmd
o3de.bat get-registered --default-folder projects
```

返回在`o3de_manifest.json`文件中注册的默认 Gems 文件夹。

```cmd
o3de.bat get-registered --default-folder gems
```

返回在`o3de_manifest.json`文件中注册的默认 templates 文件夹。

```cmd
o3de.bat get-registered --default-folder templates
```

返回在`o3de_manifest.json`文件中注册的默认受限文件夹。

```cmd
o3de.bat get-registered --default-folder restricted
```


### 可选参数

- **`-h, --help`**

  显示帮助消息。

- **`-en ENGINE_NAME, --engine-name ENGINE_NAME`**

  引擎的名称。

- **`-pn PROJECT_NAME, --project-name PROJECT_NAME`**

  项目的名称。

- **`-gn GEM_NAME, --gem-name GEM_NAME`**

  Gem 的名称。

- **`-tn TEMPLATE_NAME, --template-name TEMPLATE_NAME`**

  模板的名称。

- **`-df {engines,projects,gems,templates,restricted}, --default-folder {engines,projects,gems,templates,restricted}`**

  O3DE 中引擎、项目、Gem、模板和受限文件夹的默认文件夹。

- **`-rn REPO_NAME, --repo-name REPO_NAME`**

  存储库的名称。

- **`-rsn RESTRICTED_NAME, --restricted-name RESTRICTED_NAME`**

  受限制文件夹的名称。

- **`-ohf OVERRIDE_HOME_FOLDER, --override-home-folder OVERRIDE_HOME_FOLDER`**

  使用指定的文件夹覆盖默认主文件夹。默认情况下，主文件夹是用户文件夹。


<!-------------------------------------------------------------->

## `enable-gem`

在项目中启用指定的 Gem，以便您可以使用 Gem 提供的资产或代码。启用 Gem 时，此命令会将其名称添加到项目的`Code/enabled_gems.cmake`文件中，该文件会将 Gem 添加为项目的 **Game Launcher**、Editor 和 Asset Processor 的构建和加载依赖项。

### 格式

```cmd
enable-gem [-h] [-v] (-pp PROJECT_PATH | -pn PROJECT_NAME)
                          (-gp GEM_PATH | -gn GEM_NAME | -agp [ALL_GEM_PATHS ...]) [-f | -dry]   
                          [-o]
```

### 用法

```cmd
o3de.bat enable-gem -gp GEM_PATH -pp PROJECT_PATH
```

### 可选参数

- **`-h, --help`**

  显示帮助消息。

- **`-v`**

  其他日志记录详细程度， 可以是-v 或 -vv

- **`-pp PROJECT_PATH, --project-path PROJECT_PATH`**

  项目的路径。

- **`-pn PROJECT_NAME, --project-name PROJECT_NAME`**

  项目的名称。

- **`-gp GEM_PATH, --gem-path GEM_PATH`**

  Gem 的路径。

- **`-gn GEM_NAME, --gem-name GEM_NAME`**

  Gem 的名称（例如 “atom”）。还可以包括版本说明符，例如 "atom>=1.2.3"

- **`agp [ALL_GEM_PATHS ...], --all-gem-paths [ALL_GEM_PATHS ...]`**

  以递归方式显式激活路径中的所有 Gem。

- **`-f, --force`**

  绕过版本兼容性检查

- **`-dry, --dry-run`**

  执行试运行，报告结果而不更改任何内容。     

- **`-o, --optional`**

  将 Gem 标记为可选，以便在未找到项目时仍可配置项目。


<!-------------------------------------------------------------->

## `disable-gem`

在项目中禁用指定的 Gem。禁用 Gem 时，此命令会将其从项目的`Code/enabled_gems.cmake`文件中删除，从而将 Gem 作为项目的 Game Launcher、Editor 和 Asset Processor 的构建和加载依赖项删除。

### 格式

```cmd
disable-gem [-h] [-v] (-pp PROJECT_PATH | -pn PROJECT_NAME)
            (-gp GEM_PATH | -gn GEM_NAME | -agp [ALL_GEM_PATHS ...])
            [-egf ENABLED_GEM_FILE]
```

### 用法

```cmd
o3de.bat disable-gem -gp GEM_PATH -pp PROJECT_PATH
```

### 可选参数

- **`-h, --help`**

  显示帮助消息。

- **`-pp PROJECT_PATH, --project-path PROJECT_PATH`**

  项目的路径。

- **`-pn PROJECT_NAME, --project-name PROJECT_NAME`**

  项目的名称。

- **`-gp GEM_PATH, --gem-path GEM_PATH`**

  Gem 的路径。

- **`-gn GEM_NAME, --gem-name GEM_NAME`**

  Gem 的名称。

- **`-agp [ALL_GEM_PATHS ...], --all-gem-paths [ALL_GEM_PATHS ...]`**

  以递归方式删除路径中所有 Gem 的显式激活。

- **`-egf ENABLED_GEM_FILE, --enabled-gem-file ENABLED_GEM_FILE`**

  管理已启用 Gem 列表的 CMake 文件。禁用 Gem 时，此命令会将其从指定文件中删除。如果未指定，则此命令使用 `Code/enabled_gem.cmake` 文件。


<!-------------------------------------------------------------->

## `edit-engine-properties`

通过修改 `engine.json` 文件来编辑指定引擎的属性。

### 格式

```cmd
edit-engine-properties [-h] 
                        [-ep ENGINE_PATH | -en ENGINE_NAME]
                        [-enn ENGINE_NEW_NAME] [-ev ENGINE_VERSION]
                        [-edv ENGINE_DISPLAY_VERSION] [-agn [ADD_GEM_NAMES ...] |
                        -dgn [DELETE_GEM_NAMES ...] | -rgn
                        [REPLACE_GEM_NAMES ...]] [-aav [ADD_API_VERSIONS ...] |  
                        -dav [DELETE_API_VERSIONS ...] | -rav
                        [REPLACE_API_VERSIONS ...]]
```

### 用法

更新位于指定引擎文件夹中的 `engine.json` 文件中的`engine_name` 字段。

```cmd
o3de.bat edit-engine-properties --engine-path ENGINE_PATH --engine-new-name ENGINE_NEW_NAME

// or

o3de.bat edit-engine-properties --engine-name ENGINE_NAME --engine-new-name ENGINE_NEW_NAME
```


### 可选参数

  
- **`-h, --help`**

  显示帮助消息。

- **`-ep ENGINE_PATH, --engine-path ENGINE_PATH`**

  引擎的路径。

- **`-en ENGINE_NAME, --engine-name ENGINE_NAME`**

  引擎的名称。

- **`-agn [ADD_GEM_NAMES ...], --add-gem-names [ADD_GEM_NAMES ...]`**

  将 Gem 名称添加到gem_names字段。空格分隔列表，例如： -agn A B C`

- **`-dgn [DELETE_GEM_NAMES ...], --delete-gem-names [DELETE_GEM_NAMES ...]`**

  从 gem_names 字段中删除 Gem 名称。空格分隔列表，例如： `-dgn A B C`

- **`-rgn [REPLACE_GEM_NAMES ...], --replace-gem-names [REPLACE_GEM_NAMES ...]`**

  将整个`gem_names`字段替换为以空格分隔的值列表

- **`-aav [ADD_API_VERSIONS ...], --add-api-versions [ADD_API_VERSIONS ...]`**

  将 api 版本添加到 api_versions 字段，如果 api 条目已存在，则替换现有版本。以空格分隔的 key=value 对列表，例如： `-aav framework=1.2.3`

- **`-dav [DELETE_API_VERSIONS ...], --delete-api-versions [DELETE_API_VERSIONS ...]`**

  从 api_versions 字段中删除 api 条目。空格分隔列表，例如： `-dav framework`

- **`-rav [REPLACE_API_VERSIONS ...], --replace-api-versions [REPLACE_API_VERSIONS ...]`**

  将整个字段替换为以空格分隔的 key=value 对列表api_versions例如： `-rav framework=1.2.3`


**引擎属性：**

以下参数修改指定引擎的属性。
  
- **`-enn ENGINE_NEW_NAME, --engine-new-name ENGINE_NEW_NAME`**

  设置引擎的名称。

- **`-ev ENGINE_VERSION, --engine-version ENGINE_VERSION`**

  设置引擎的版本。

- **`-edv ENGINE_DISPLAY_VERSION, --engine-display-version ENGINE_DISPLAY_VERSION`**

  设置引擎的显示版本。


<!-------------------------------------------------------------->

## `edit-project-properties`

通过修改 `project.json` 文件来编辑指定项目的属性。

### 格式 

```cmd
edit-project-properties [-h] 
                        (-pp PROJECT_PATH | -pn PROJECT_NAME)
                        [-pv PROJECT_VERSION] [-pnn PROJECT_NEW_NAME]
                        [-pid PROJECT_ID] [-en ENGINE_NAME]
                        [-efcp ENGINE_FINDER_CMAKE_PATH] [-ep ENGINE_PATH]    
                        [-po PROJECT_ORIGIN] [-pd PROJECT_DISPLAY]
                        [-ps PROJECT_SUMMARY] [-pi PROJECT_ICON] [--user]     
                        [-at [ADD_TAGS ...] | -dt [DELETE_TAGS ...] | -rt     
                        [REPLACE_TAGS ...]] [-agn [ADD_GEM_NAMES ...] | -dgn  
                        [DELETE_GEM_NAMES ...] | -rgn [REPLACE_GEM_NAMES ...]]
                        [-aev [ADD_COMPATIBLE_ENGINES ...] | 
                        -dev [DELETE_COMPATIBLE_ENGINES ...] | 
                        -rev [REPLACE_COMPATIBLE_ENGINES ...]]
                        [-aav [ADD_ENGINE_API_DEPENDENCIES ...] | 
                        -dav [DELETE_ENGINE_API_DEPENDENCIES ...] | 
                        -rav [REPLACE_ENGINE_API_DEPENDENCIES ...]]
```

### 用法

更新位于提供的项目路径或已注册项目路径中的`gem.json`文件中的`project_name`字段。

```cmd
o3de.bat edit-project-properties --project-path PROJECT_PATH --project-new-name PROJECT_NEW_NAME
o3de.bat edit-project-properties --project-name PROJECT_NAME --project-new-name PROJECT_NEW_NAME
```

### 可选参数

- **`-h, --help`**

  显示帮助消息。

- **`-pp PROJECT_PATH, --project-path PROJECT_PATH`**

  项目的路径。

- **`-pn PROJECT_NAME, --project-name PROJECT_NAME`**

  项目的名称。

- **`-at [ADD_TAGS [ADD_TAGS ...]], --add-tags [ADD_TAGS [ADD_TAGS ...]]`**

  向 `user_tags` 属性添加标记。要添加多个标签，请使用以空格分隔的列表。例如： `-at A B C`.

- **`-dt [DELETE_TAGS [DELETE_TAGS ...]], --delete-tags [DELETE_TAGS [DELETE_TAGS ...]]`**

  从 `user_tags` 属性中删除标记。要删除多个标签，请使用以空格分隔的列表。例如： `-dt A B C`.

- **`-rt [REPLACE_TAGS [REPLACE_TAGS ...]], --replace-tags [REPLACE_TAGS [REPLACE_TAGS ...]]`**

  将 `user_tags` 属性替换为指定的以空格分隔的值列表。

- **`-agn [ADD_GEM_NAMES ...], --add-gem-names [ADD_GEM_NAMES ...]`**

  将 Gem 名称添加到gem_names字段。空格分隔列表，例如： `-agn A B C`

- **`-dgn [DELETE_GEM_NAMES ...], --delete-gem-names [DELETE_GEM_NAMES ...]`**

  从 gem_names 字段中删除 Gem 名称。空格分隔列表，例如： `-dgn A B C`

- **`-rgn [REPLACE_GEM_NAMES ...], --replace-gem-names [REPLACE_GEM_NAMES ...]`**

  将整个字段替换为以空格分隔的值列表gem_names

- **`-aev [ADD_COMPATIBLE_ENGINES ...], --add-compatible-engines [ADD_COMPATIBLE_ENGINES ...]`**       

  添加与此项目兼容的引擎版本。空格分隔列表，例如： `-aev o3de>=1.2.3 o3de-sdk~=2.3`.

- **`-dev [DELETE_COMPATIBLE_ENGINES ...], --delete-compatible-engines [DELETE_COMPATIBLE_ENGINES ...]`**

  从 compatible_engines 属性中删除引擎版本。空格分隔列表，例如： `-dev o3de>=1.2.3 o3de-sdk~=2.3`.

- **`-rev [REPLACE_COMPATIBLE_ENGINES ...], --replace-compatible-engines [REPLACE_COMPATIBLE_ENGINES ...]`**

  将整个字段替换为compatible_engines空格分隔的值列表。

- **`-aav [ADD_ENGINE_API_DEPENDENCIES ...], --add-engine-api-dependencies [ADD_ENGINE_API_DEPENDENCIES ...]`**

  添加与此 Gem 兼容的引擎 api 依赖项。可以多次指定。

- **`-dav [DELETE_ENGINE_API_DEPENDENCIES ...], --delete-engine-api-dependencies [DELETE_ENGINE_API_DEPENDENCIES ...]`**

  从 compatible_engines 属性中删除引擎 API 依赖项。   
  可以多次指定。

- **`-rav [REPLACE_ENGINE_API_DEPENDENCIES ...], --replace-engine-api-dependencies [REPLACE_ENGINE_API_DEPENDENCIES ...]`**

  替换 compatible_engines 属性中的引擎 API 依赖项。可以多次指定。

**项目属性：**

以下参数修改指定工程的属性。

- **`-pv PROJECT_VERSION, --project-version PROJECT_VERSION`**

  设置项目版本。

- **`-pnn PROJECT_NEW_NAME, --project-new-name PROJECT_NEW_NAME`**

  设置项目的名称。

- **`-pid PROJECT_ID, --project-id PROJECT_ID`**

  设置项目的 ID。

- **`-en ENGINE_NAME, --engine-name ENGINE_NAME`**

  设置项目的引擎名称。

- **`-efcp ENGINE_FINDER_CMAKE_PATH, --engine-finder-cmake-path ENGINE_FINDER_CMAKE_PATH`**

  设置此项目的引擎查找器 cmake 文件的路径。

- **`-ep ENGINE_PATH, --engine-path ENGINE_PATH`**

  设置项目的引擎路径。
{{< note >}}
  此设置仅允许使用`--user`参数，以避免将本地路径添加到共享的 `project.json`
{{< /note >}}

- **`-po PROJECT_ORIGIN, --project-origin PROJECT_ORIGIN`**

  设置项目来源的描述或 URL（例如项目主机、存储库、所有者...等）。

- **`-pd PROJECT_DISPLAY, --project-display PROJECT_DISPLAY`**

  设置项目显示名称。

- **`-ps PROJECT_SUMMARY, --project-summary PROJECT_SUMMARY`**

  设置项目的摘要描述。

- **`-pi PROJECT_ICON, --project-icon PROJECT_ICON`**

  设置 projects 图标资源的路径。

- **`--user`**

  仅对 `<project>/user/project.json` 进行更改。这对于在本地覆盖`<project>/project.json`中共享的设置很有用。

- **`-pnn PROJECT_NEW_NAME, --project-new-name PROJECT_NEW_NAME`**

  设置项目的名称。

- **`-po PROJECT_ORIGIN, --project-origin PROJECT_ORIGIN`**

  设置项目源的描述或 URL，例如项目主机、存储库或所有者。

- **`-pd PROJECT_DISPLAY, --project-display PROJECT_DISPLAY`**

  设置项目的显示名称。

- **`-ps PROJECT_SUMMARY, --project-summary PROJECT_SUMMARY`**

  设置项目的摘要描述。

- **`-pi PROJECT_ICON, --project-icon PROJECT_ICON`**

  设置项目的 icon 资源的路径。

<!-------------------------------------------------------------->

## `edit-gem-properties`

通过修改`gem.json`文件来编辑指定 Gem 的属性。

### 格式

```cmd
edit-gem-properties [-h] 
                    (-gp GEM_PATH | -gn GEM_NAME) [-gnn GEM_NEW_NAME]        
                    [-gd GEM_DISPLAY] [-go GEM_ORIGIN] [-gt {Code,Tool,Asset}]    
                    [-gs GEM_SUMMARY] [-gi GEM_ICON] [-gr GEM_REQUIREMENTS]       
                    [-gdu GEM_DOCUMENTATION_URL] [-gl GEM_LICENSE]
                    [-glu GEM_LICENSE_URL] [-gv GEM_VERSION]
                    [-aev [ADD_COMPATIBLE_ENGINES ...] | 
                    -dev [REMOVE_COMPATIBLE_ENGINES ...] | 
                    -rev [REPLACE_COMPATIBLE_ENGINES ...]]
                    [-aav [ADD_ENGINE_API_DEPENDENCIES ...] | 
                    -dav [REMOVE_ENGINE_API_DEPENDENCIES ...] | 
                    -rav [REPLACE_ENGINE_API_DEPENDENCIES ...]] 
                    [-at [ADD_TAGS ...] | -dt [REMOVE_TAGS ...] | -rt [REPLACE_TAGS ...] | 
                    -apl [ADD_PLATFORMS ...] | -dpl [REMOVE_PLATFORMS ...] | -rpl [REPLACE_PLATFORMS ...]]
```

### 用法

更新位于指定 Gem 文件夹中的 `gem.json`文件中的 `gem_name` 字段。

```cmd
o3de.bat edit-gem-properties --gem-path GEM_PATH --gem-new-name GEM_NEW_NAME

// or

o3de.bat edit-gem-properties --gem-name GEM_NAME --gem-new-name GEM_NEW_NAME
```

### 可选参数

- **`-h, --help`**
  
  显示帮助消息。

- **`-gp GEM_PATH, --gem-path GEM_PATH`**

  Gem 的路径。

- **`-gn GEM_NAME, --gem-name GEM_NAME`**

  Gem 的名称。

- **`-aev [ADD_COMPATIBLE_ENGINES ...], --add-compatible-engines [ADD_COMPATIBLE_ENGINES ...]`**

  添加与此 Gem 兼容的引擎版本。可以多次指定。

- **`  -dev [REMOVE_COMPATIBLE_ENGINES ...], --remove-compatible-engines [REMOVE_COMPATIBLE_ENGINES ...]`**

  从 compatible_engines 属性中删除引擎版本。可以多次指定。

- **`  -rev [REPLACE_COMPATIBLE_ENGINES ...], --replace-compatible-engines [REPLACE_COMPATIBLE_ENGINES ...]`**

  替换 compatible_engines 属性中的引擎版本。可以多次指定。

- **`  -aav [ADD_ENGINE_API_DEPENDENCIES ...], --add-engine-api-dependencies [ADD_ENGINE_API_DEPENDENCIES ...]`**

  添加与此 Gem 兼容的引擎 api 依赖项版本。可以多次指定。

- **`  -dav [REMOVE_ENGINE_API_DEPENDENCIES ...], --remove-engine-api-dependencies [REMOVE_ENGINE_API_DEPENDENCIES ...]`**

  从 compatible_engines 属性中删除引擎 API 依赖项版本。可以多次指定。

- **`  -rav [REPLACE_ENGINE_API_DEPENDENCIES ...], --replace-engine-api-dependencies [REPLACE_ENGINE_API_DEPENDENCIES ...]`**

  替换 compatible_engines 属性中的引擎 API 依赖项。  
  可以多次指定。

- **`-at [ADD_TAGS [ADD_TAGS ...]], --add-tags [ADD_TAGS [ADD_TAGS ...]]`**

  向 `user_tags` 属性添加标记。要添加多个标签，请使用以空格分隔的列表。例如： `-at A B C`.

- **`-dt [DELETE_TAGS [DELETE_TAGS ...]], --delete-tags [DELETE_TAGS [DELETE_TAGS ...]]`**

  从 `user_tags` 属性中删除标记。要删除多个标签，请使用以空格分隔的列表。例如： `-dt A B C`.

- **`-rt [REPLACE_TAGS [REPLACE_TAGS ...]], --replace-tags [REPLACE_TAGS [REPLACE_TAGS ...]]`**
  
  将 `user_tags` 属性替换为指定的以空格分隔的值列表。

- **`-apl [ADD_PLATFORMS ...], --add-platforms [ADD_PLATFORMS ...]`**

  将 platform（s） 添加到 platforms 属性。可以多次指定。

- **`-dpl [REMOVE_PLATFORMS ...], --remove-platforms [REMOVE_PLATFORMS ...]`**

  从 platforms 属性中删除 platform（s）。可以多次指定。

- **`-rpl [REPLACE_PLATFORMS ...], --replace-platforms [REPLACE_PLATFORMS ...]`**

  替换 platforms 属性中的 platform（s）。可以多次指定。

**Gem 属性:**

  以下参数修改指定 Gem 的属性。

- **`-gnn GEM_NEW_NAME, --gem-new-name GEM_NEW_NAME`**

  设置 Gem 的名称。

- **`-gd GEM_DISPLAY, --gem-display GEM_DISPLAY`**

  设置 Gem 的显示名称。

- **`-go GEM_ORIGIN, --gem-origin GEM_ORIGIN`**

  设置 Gem 源的描述或 URL，例如 Gem 主机、存储库或所有者。

- **`-gt {Code,Tool,Asset}, --gem-type {Code,Tool,Asset}`**

  将 Gem 类型设置为 Code、Tool 或 Asset。

- **`-gs GEM_SUMMARY, --gem-summary GEM_SUMMARY`**

  设置 Gem 的摘要描述。

- **`-gi GEM_ICON, --gem-icon GEM_ICON`**

  设置 Gem 的 icon 资源的路径。

- **`-gr GEM_REQUIREMENTS, --gem-requirements GEM_REQUIREMENTS`**

  设置使用 Gem 所需的要求的描述。

- **`-gdu GEM_DOCUMENTATION_URL, --gem-documentation-url GEM_DOCUMENTATION_URL`**

  设置 Gem 文档的 URL。

- **`-gl GEM_LICENSE, --gem-license GEM_LICENSE`**

  设置 Gem 许可证的名称。

- **`-glu GEM_LICENSE_URL, --gem-license-url GEM_LICENSE_URL`**

  设置 Gem 许可证的 URL。

- **`-gv GEM_VERSION, --gem-version GEM_VERSION`**

  设置 Gem 的版本。

<!-------------------------------------------------------------->

## `download`

从远程存储库下载引擎、项目、Gem 或模板。

### 格式

```cmd
o3de.py download [-h]
              (--engine-name ENGINE_NAME | --project-name PROJECT_NAME | --gem-name GEM_NAME | --template-name TEMPLATE_NAME)
              [--dest-path DEST_PATH] [--skip-auto-register] [--force]
              [--use-source-control]
```

### 用法

```cmd
o3de.bat download --project-name "CustomProject" --dest-path "C:/projects"
```
将导致`CustomProject`下载到 `C:/projects/CustomProject`。
如果未提供`--dest-path`，则`CustomProject`将下载到默认项目文件夹。

```cmd
o3de.bat download --gem-name "CustomGem==2.0.0"
```
将下载名为`CustomGem` 版本 `2.0.0`的 Gem（如果可用）。
如果仅提供 Gem 名称，则将下载可用的最高版本。

### 可选参数

- **`-h, --help`**

  显示帮助消息并退出

- **`--engine-name ENGINE_NAME, -e ENGINE_NAME`**

  可下载的引擎名称。

- **`--project-name PROJECT_NAME, -p PROJECT_NAME`**

  带有可选版本说明符的可下载项目名称，例如 `project==1.2.3`。 如果未提供版本说明符，则将下载最新版本。

- **`--gem-name GEM_NAME, -g GEM_NAME`**

  带有可选版本说明符的可下载 Gem 名称，例如`gem==1.2.3`。 如果未提供版本说明符，则将下载最新版本。

- **` --template-name TEMPLATE_NAME, -t TEMPLATE_NAME`**

  带有可选版本说明符的可下载模板名称，例如`template==1.2.3`。 如果未提供版本说明符，则将下载最新版本。

- **` --dest-path DEST_PATH, -dp DEST_PATH`**

  要下载到的可选目标文件夹。

- **` --skip-auto-register, -sar`**

  跳过新对象下载的自动注册

- **` --force, -f`**

  强制覆盖当前对象

- **` --use-source-control, --src`**

  从源代码控制获取，而不是下载 `.zip`存档。      
  要求对象具有有效的`source_control_uri`。

<!-------------------------------------------------------------->
## `repo`

更新元数据，激活和停用远程存储库。

### 格式

```cmd
repo [-h] [-ar ACTIVATE_REPO | -dr DEACTIVATE_REPO | -r REFRESH_REPO | -ra] 
```

### 用法

```cmd
o3de.bat repo --refresh-all-repos
```
更新所有已知远程仓库的元数据。

```cmd
o3de.bat repo --deactivate-repo https://github.com/o3de/example
```
停用`https://github.com/o3de/example` 远程存储库，以便其内容不再显示在搜索中，也不再可供下载。

### 可选参数

- **`-ar ACTIVATE_REPO, --activate-repo ACTIVATE_REPO`**

  激活指定的远程存储库，允许从中搜索和下载对象。
                        
- **`-dr DEACTIVATE_REPO, --deactivate-repo DEACTIVATE_REPO`**

  停用指定的远程存储库，阻止搜索或从中下载任何对象。

- **`-r REFRESH_REPO, --refresh-repo REFRESH_REPO`**

  获取指定远程存储库的最新元数据。

- **`-ra, --refresh-all-repos`**

  从所有已知的远程仓库中获取最新的元数据。

<!-------------------------------------------------------------->

## `sha256`

使用 SHA-256（安全哈希算法 256）为 O3DE 对象创建哈希值。此命令输出指定的文件路径，并将该值写入指定 JSON 文件的 `sha256` 字段。

### 格式

```cmd
sha256 [-h] -f FILE_PATH [-j JSON_PATH]
```

### 用法

```cmd
o3de.bat sha256 --file-path FILE_PATH --json-path JSON_PATH
```

### 可选参数

- **`-f FILE_PATH, --file-path FILE_PATH`**

  O3DE 对象的路径。

- **`-j JSON_PATH, --json-path JSON_PATH`**

  要将`sha256`哈希值添加到的 O3DE 对象的 JSON 文件的路径。

<!-------------------------------------------------------------->

## `android-configure`

管理 Android 项目生成过程用于创建 Android Gradle 构建脚本的设置。

### 格式

```cmd
android-configure [-h] [--global] [-p PROJECT] [-l] [--validate] [--set-value VALUE] [--clear-value VALUE] [--set-password SETTING] [--debug]
```

### 用法

```cmd
android-configure -l
```
这将列出 Android Project Generation 脚本将使用的当前设置。

```cmd
android-configure --validate
```
这将执行试运行，并验证是否配置了最低设置以及是否满足先决条件软件环境。否则，它将报告详细错误。

```cmd
android-configure --set-value SETTING
```
这将根据 **SETTING** 相等表达式设置 Android 设置。**SETTING** 格式必须采用`<setting>=<value>` 的形式，其中 **\<setting\>** 是要设置的 Android 设置，**\<value\>** 是要设置的值。请注意，密码设置不能通过此参数设置，必须使用 `--set-password` 代替。

```cmd
android-configure --clear-value SETTING
```
这将清除特定的 android 设置 **SETTING**。这也可用于清除密码设置。

```cmd
android-configure --set-password SETTING
```
这将通过标准密码设置和验证提示设置密码 android 设置 **SETTING**。

### 可选参数

- **`--help`**

  显示此命令的标准使用文档，以及 android 设置及其说明的列表。

- **`-p PROJECT`**

  尝试为注册为 **\<project\>** 的项目查找并应用任何特定于项目的设置。**android-configure** 命令的默认行为是尝试根据当前工作目录检测项目。

- **`--global`**

  对全局值应用 **android-configure** 命令。全局值将应用于没有仅应用于该项目的特定值的所有项目。默认行为是尽可能将命令应用于本地项目。

- **`--debug`**

  在运行此命令时启用更详细的调试消息

<!-------------------------------------------------------------->

## `android-generate`

为您的项目生成 Android Gradle 项目。特定项目详细信息通过由 `android-configure`命令管理的 android 特定设置进行配置。

### 格式

```cmd
android-generate [-h] -p PROJECT -B BUILD_DIR [--platform-sdk-api-level PLATFORM_SDK_API_LEVEL] [--ndk-version NDK_VERSION] [--signconfig-store-file SIGNCONFIG_STORE_FILE] [--signconfig-key-alias SIGNCONFIG_KEY_ALIAS] [--asset-mode ASSET_MODE] [--extra-cmake-args EXTRA_CMAKE_ARGS] [--custom-jvm-args CUSTOM_JVM_ARGS] [--strip-debug] [--oculus-project] [--debug]
```

### 用法

```cmd
android-generate -p <project> -B <build-dir>
```
这将在目录 **\<build-dir\>>** 中为 **\<project\** 生成一个 Android Gradle 项目。**\<project\>** 可以是 O3DE 项目的路径，也可以是已注册项目的名称。


### 可选参数

- **`--help`**

  显示此命令的标准用法文档。

- **`--platform-sdk-api-level PLATFORM_SDK_API_LEVEL`**

  指定特定的 [Android 平台 SDK API 级别](https://developer.android.com/tools/releases/platforms). 默认 API 级别由`platform.sdk.api`设置控制。

- **`--ndk-version NDK_VERSION`**

  指定特定的 [Android NDK](https://developer.android.com/ndk) 到 [download](https://developer.android.com/ndk/downloads) 并用于构建本机代码。**NDK_VERSION** 的值表示版本号，而不是版本。支持通配符，因此您无需提供整个版本。例如，您可以使用 `25.*` 搜索并获取修订版 **r25c** 的最新主要版本。默认 NDK 版本由`ndk.version` 设置控制。

- **`--signconfig-store-file SIGNCONFIG_STORE_FILE`**

  指定可选的 Android 签名配置密钥存储文件。如果在设置中设置了`signconfig.store.file`，它将用作默认值。

- **`--signconfig-key-alias SIGNCONFIG_KEY_ALIAS`**

  在密钥存储文件中指定可选的 Android 签名配置密钥别名。如果在设置中设置了`signconfig.key.alias`，它将被用作默认值。

- **`--asset-mode ASSET_MODE`**

  指定构建 APK 时要使用的资产部署方法。接受的值为 ：

  - **LOOSE**
     Loose assets （松散资源） 单个编译的资源文件。

  - **PAK**
     捆绑的发布文件是压缩为发布 Pak 文件的捆绑资产。有关如何创建捆绑资源的信息，请参阅 [捆绑项目资源](/docs/user-guide/asset-bundler/bundle-assets-for-release/) 。

    `asset.mode` 设置控制 Asset Deployment Mode （资产部署模式） 的默认值。

- **`--extra-cmake-args EXTRA_CMAKE_ARGS`**

    可选字符串，用于在 android gradle 构建过程中的本机项目生成期间设置其他 cmake 参数。此值将直接附加到 **CMake** 项目生成命令中，并可用于控制任何自定义 O3DE cmake 变量。

    `extra.cmake.args` 设置 控制此选项的默认值。
- **`--custom-jvm-args CUSTOM_JVM_ARGS`**

    自定义了调用 gradle 时要设置的 jvm 参数。此选项可用于在启动 Gradle 时调整 [JVM 内存](https://docs.gradle.org/current/userguide/config_gradle.html#sec:configuring_jvm_memory)设置。

    `gradle.jvmargs` 设置 控制此选项的默认值。

- **`--strip-debug/--no-strip-debug`**

    Flag 设置 cmake 本机生成规则，以选择性地从生成的二进制文件中去除调试符号。如果设置`strip.debug`设置为 **False**，则`--strip-debug`可用作覆盖和启用调试符号剥离的选项。如果设置为 **True**，则`--no-strip-debug`可用作覆盖和禁用符号剥离的选项。

- **`--oculus-project/--no-oculus-project`**

    用于在 Android Gradle 项目中启用或禁用 oculus 特定设置的标志。如果设置`oculus.project`设置为 **False**，则`--oculus-project` 可用于启用 oculus 特定设置。如果设置为 **True**，则`--no-oculus-project`可用于禁用 oculus 特定设置。

- **`--debug`**

  在运行此命令时启用更详细的调试消息
