---
linkTitle: 资产缓存服务器模式
title: 资产缓存服务器
description: 启用一项功能，减少团队的资产构建。
weight: 800
toc: true
---

**资产缓存服务器（ACS）** 模式允许**资产处理器**将产品资产文件缓存到远程目录，以便在团队中共享。

ACS 模式允许资产处理器从资产服务器缓存中获取作业的预处理产品，而不是在本地进行处理。具体做法是使用一个共享目录，在该目录中，资产处理器为已处理的源资产作业写出产品资产归档 ZIP 文件。资产处理器客户端检索这些 ZIP 文件并解压产品资产，而不是从头开始处理源资产。

# 在 ACS 模式下设置资产处理器

设置 ACS 模式以便开发人员使用它包括三个部分：设置 ACS 服务器、设置 ACS 客户端和配置 ACS 块。

{{< note >}}
接下来的章节假定根文件夹是 `T:/o3de`，但它可以在任何文件夹中，也可以在 Linux 上运行。
{{< /note >}}

## 创建传输目录

团队需要创建一个所有成员都能通过网络访问的共享目录。ACS 可在 Windows 和 Linux 之间运行，但  `cacheServerAddress`字符串只接受一个字符串，如 `T:/o3de/Transfer/Cache`，所有客户端都要从该字符串读取数据。可以让资产处理器在 ACS 模式下通过 Windows 向一个配置好的 `cacheServerAddress` 字符串写入数据，并让另一个 ACS 从 Linux 上的 Samba 链接中检索数据。您团队的 IT 经理可以为这种情况提供解决方案。

## 使用共享缓存选项卡

资产处理器有一个选项卡，用于管理共享缓存（又名资产缓存服务器）的设置。

面板分为三个步骤：
1. 通过 “Set shared cache mode设置共享缓存模式 ”选择模式
2. 通过 “Select a remote folder选择远程文件夹 ”选择传输目录
3. 通过 “Manage shared cache patterns管理共享缓存模式 ”选择要缓存的资产模式

![Create a log tab in Asset Processor](/images/user-guide/assets/asset-processor/acs_snapshot.png)

### 选择模式

Shared Cache共享缓存模式可设置为
 1. Inactive - 共享缓存未激活。所有资产都将在本地处理。
 2. Server - 资产处理器会将产品资产存档到远程文件夹中。
 3. Client - 资产处理器将尝试从远程文件夹检索存档产品资产。

系统默认为 “未激活”。

### 选择传输目录

该字段应包含在 “创建传输目录Create the transfer directory”步骤中设置的远程文件夹的完整路径。点击 “Save Changes保存更改 ”按钮后，系统将检测传输目录的有效性。

### 管理共享缓存模式

该表显示在服务器上处理资产时将缓存的活动资产模式。资产模式行包含：
1. 复选框用于启用或禁用模式
2. 一个文本框，用于存储模式名称
3. 如何使用模式匹配源资产的组合框（通配符或 RegEx）
4. 一个文本框，用于输入匹配源资产的模式
5. 删除模式的按钮

启用标志允许用户将该行切换为禁用，以强制本地客户端资产处理器处理匹配的资产。大多数情况下，客户端用户会启用所有模式。

`Name`是一个标签，用于解释正在匹配的源资产。如果名称与资产创建器的名称相匹配，则使用该资产创建器的源资产将缓存产品存档。

`Type`是 `Wildcard` 或 `RegEx`，它告诉系统如何使用 `Pattern`文本来匹配源资产文件。`Wildcard` 通常以星号 (\*) 开头，后面跟一个点 (.) 和一个扩展名，用于匹配所有具有特定扩展名的源资产，如 `*.png` 和 `*.wav`，分别用于匹配 PNG 和 WAV 文件。`RegEx`类型将`Pattern`解释为正则表达式，因此项目可以指定更高级的匹配，如子文件夹或多种资产类型。

垃圾桶 按钮可用于删除现有资产模式行。"+ Add Pattern"可用于添加新的资产模式行。

### 保存或丢弃按钮

当检测到设置更改时，`Save Changes`和 `Discard`按钮就会启用。`Save Changes`按钮会将更改提交到设置文件中。 `Discard` 按钮会恢复面板的值。

设置文件将被写入`{project_folder}/Registry/asset_cache_server_settings.setreg`文件。

如果远程文件夹设置为无效文件夹位置，则会显示错误对话框，并且不会写入任何设置。

# 资产缓存服务器： 设置详情

资产缓存服务器的设置保存在项目内的 `.setreg` 文件中。AutomatedTesting 项目的资产缓存服务器默认设置保存在`o3de/AutomatedTesting/Registry/asset_cache_server_settings.setreg` 文件中。

该文件存储一个 JSON 文档，其中有一个`/AssetProcessor/Settings/Server`键，包括至少两个`assetCacheServerMode`和`cacheServerAddress`键，用于设置资产缓存服务器模式。它还可以包含任意数量的`ACS`键，用于管理要缓存的资产模式。

`cacheServerAddress` 键存储一个字符串字段，其中包含远程文件夹的目录名。

`assetCacheServerMode` 键存储一个字符串，用于设置资产缓存服务器的模式。可将其设置为`inactive`（默认值），这样 AP 就不会缓存任何文件（无论是存储还是检索）。`server`模式用于存档产品资产文件。`client`模式检索产品存档文件。

可以有任意多个前缀为 `ACS` 的键来存储资产模式，如 `ACS 音频文件` 和 `ACS PNG 文件`。`ACS 块`的结构详见 [`配置 ACS 块`](#configuring-an-acs-block)。

## 在 ACS 模式下将资产处理器配置为服务器

ACS 模式的设计是让一台机器为远程目录中的资产缓存做贡献，其他资产处理器客户端检索缓存的产品资产存档文件。

要在 ACS 模式下将资产处理器作为服务器启用，`.setreg` 文件需要这些设置：
```json
{
    "Amazon": {
        "AssetProcessor": {
            "Settings": {
                "Server": {
                    "cacheServerAddress": "T:/o3de/cache_server",
                    "assetCacheServerMode": "server",
                    "ACS Atom Image Builder": {
                        "name": "Atom Image Builder",
                        "glob": "*.png",
                        "checkServer": true
                    },
                    "ACS Precompiled Shader Builder": {
                        "name": "Precompiled Shader Builder",
                        "glob": "*.precompiledshader",
                        "checkServer": true
                    },
                    "ACS Scene Builder": {
                        "name": "Scene Builder",
                        "glob": "*.fbx",
                        "checkServer": true
                    },
                    "ACS Shader Asset Builder": {
                        "name": "Shader Asset Builder",
                        "glob": "*.shader",
                        "checkServer": true
                    }
                }
            }
        }
    }
}
```

上例启用了 Asset Processor 的 ACS 模式，这样它就会将所有 FBX 文件的缓存归档文件写入 `T:/o3de/cache_folder`。

设置键 `/AssetProcessor/Settings/Server/assetCacheServerMode` 设置资产缓存服务器的模式。本例中的 `assetCacheServerMode` 设置为 `server`。

设置键 `/AssetProcessor/Settings/Server/cacheServerAddress=<remote_shared_path>` 指向一个远程目录，服务器和所有客户端都可以从中读取和写入。传输目录应在启动 Asset Processor 之前设置好。本示例将`<remote_shared_path>`设置为`T:/o3de/cache_server`。

设置键 `/AssetProcessor/Settings/Server/ACS FBX Glob={}` 对象将 FBX 源资产指定为要缓存的文件类型。在设置注册表中可以指定许多条目，其中条目的标题需要以 ACS 字母开头，如 “ACS Our Textures（ACS 我们的纹理）”和 “ACS Audio Files（ACS 音频文件）”。"glob"模式可用于通过扩展名或一些基本的匹配模式来捕获文件。使用`"checkServer": true`标记条目以启用缓存非常重要。

## 以客户端模式配置 ACS 模式下的资产处理器

要在 ACS 模式下以客户端身份运行资产处理器，客户端机器需要访问远程目录，在客户端模式下启用缓存系统，并指定从远程服务器提取的资产文件类型。

```json
{
    "Amazon": {
        "AssetProcessor": {
            "Settings": {
                "Server": {
                    "cacheServerAddress": "T:/o3de/cache_server",
                    "assetCacheServerMode": "client"
                }
            }
        }
    }
}
```

设置键`/AssetProcessor/Settings/Server/assetCacheServerMode="client"`可使资产处理器以`client`模式运行。在这种模式下，Asset Processor 会读取源资产更改，检查远程目录中的产品存档，并在未找到时处理资产。由于团队成员会先在本地添加新资产，然后再将其提交到远程源资产库，因此可以混合使用远程和本地源资产。

{{< note >}}
缓存服务器地址应与 ACS 服务器相匹配。
{{< /note >}}

## 配置 ACS 块

资产缓存系统是使用选择模式配置的。有许多类型的文件比从远程文件夹复制存档文件处理得更快。缓存源资产文件产品的最常见方法是使用`"glob"`通配符模式，如 `"*.png"` 和 `"*.wav"` 扫描模式。另一种方法是添加一个正则表达式来匹配使用`"pattern"`需要长时间处理的源资产，如`"\/assets\/rock_[\w]*\.asset"`来缓存所有的岩石资产文件。

```
"ACS title":
{
   "name": (string) a label for this block, can be used to match any builder by name
   "glob":  (string) wild card pattern i.e. "*.fbx"
   "pattern": (string) a regular expression i.e. "[\w]*\.asset"
   "checkServer": (Boolean) to enable set to true
}
```

此 ACS 块允许用户配置应缓存在远程文件夹中的源资产类型。该块位于 JSON 路径`/AssetProcessor/Settings/Server`对象中。标题必须以`"ACS "`前缀开头，以指定该对象为配置块。下一部分是`"glob"` 或 `"pattern"`，后面是通配符模式或正则表达式的正确文本；这些用于标记需要缓存的源资产。

`"name"`字段用于 GUI 工具中配置块的标题，但也可以通过将`"name"`设置为资产生成器的名称来匹配资产模式。例如，配置块可以将名称设为 “Atom Image Builder”，这样所有处理过的图像都会被缓存。、

{{< note >}}
块只能设置为`"glob"` 或 `"pattern"`，而不能同时设置为`"glob"` 和 `"pattern"`。
{{< /note >}}

`"checkServer"`布尔标志用于启用代码块。`"checkServer"`的默认值是 false，因此要启用 ACS 代码块，需要将布尔标志设置为 true。

例如，此 JSON ACS 块将缓存由"XmlBuilderWorker" 创建器创建的所有产品资产：

```json
"ACS XmlBuilderWorker":
{
   "name": "XmlBuilderWorker",
   "glob":  "*.vegdescriptorlist",
   "checkServer": true
}
```
