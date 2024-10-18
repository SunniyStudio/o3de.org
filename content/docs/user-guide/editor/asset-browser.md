---
linkTitle: Asset Browser
title: Asset Browser
description: 使用 Open 3D Engine (O3DE) 中的Asset Browser管理资产。
weight: 400
---

**Asset Browser**显示 Open 3D Engine (O3DE) 和您的 O3DE 项目中的所有资产。您可以查看和管理资产，在不同的查看模式之间切换，浏览目录，并创建搜索过滤器。

## 打开 Asset Browser

1. 在[O3DE Editor](/docs/user-guide/editor/)中，点击 **Tools** > **Asset Browser*。

1. 在Asset Browser中，你可以创建、重命名、删除、复制、移动、选择、打开、搜索和过滤资产。

     ![Asset Browser startup view.](/images/user-guide/assetbrowser/asset-browser-welcome.png)

    - 要选择多个资产，请同时在一个资产上按**Ctrl+LMB**，使其成为多选的一部分。

    - 要打开多个**Asset Browser窗口**，**右键单击**资产，弹出右键菜单，选择**Open in another Asset Browser**。

## 查看资产

在**Asset Browser**中查看资产时，可以在**列表**、**缩略图**和**表格**视图之间切换。

### 列表视图

要切换到列表视图，请单击**List View**按钮。

![Asset Browser List View Button.](/images/user-guide/assetbrowser/list-view-button.png)

这是三个视图中最简单的单列视图。它是一个扁平、可滚动的长列表，显示所有资产。

![Asset Browser List View.](/images/user-guide/assetbrowser/list-view.png)

### 缩略图视图

要切换到缩略图视图，请单击**Thumbnail View**按钮。

![Asset Browser Thumbnail View Button.](/images/user-guide/assetbrowser/thumbnail-view-button.png)

这是一个显示资产缩略图的双列视图。第一列是一个列表，只显示文件夹，便于进行目录导航，第二列显示缩略图，以便快速查看资产预览。单击资产的切换箭头可展开该资产并显示其子资产。

![Asset Browser Thumbnail View.](/images/user-guide/assetbrowser/thumbnail-view.png)

### 表格视图

要切换到表格视图，请单击**Table View**按钮。
    
![Asset Browser Table View Button.](/images/user-guide/assetbrowser/table-view-button.png)

这是一个双列视图，在表格网格中显示资产。第一列是一个列表，只显示文件夹，以方便目录导航，第二列显示一个表格网格，其中包含有关资产的更多信息。在这一列中，您可以查看资产的**名称**、**类型**、**磁盘大小**、**顶点**和**大致世界大小**。

![Asset Browser Table View.](/images/user-guide/assetbrowser/table-view.png)

## 管理资产

在Asset Browser中，可以创建、重命名、删除、复制、移动、打开和拖放资产。这些操作在所有 “Asset Browser ”视图中都可用。

### 创建资产
通过以下操作之一创建新资产：

- **右键单击**文件夹打开右键菜单，选择**Create**。

- 在第一列中选择一个文件夹，然后单击**Create New (+)** 按钮。

![Asset Browser Folder Management.](/images/user-guide/assetbrowser/folder-context-menu.png)

### 删除资产

通过以下操作之一删除资产：

- **右键单击**资产，弹出右键菜单，选择**Delete Asset**。
- 选择资产并按**Delete**。

### 重命名资产

重命名资产的方法如下：

- **右键单击**资产，弹出右键菜单，选择**Rename Asset**。
- 选择资产并按**F2**。

### 复制资产

通过以下操作之一复制资产：

- **右键单击**资产，弹出右键菜单，选择**Duplicate Asset**。
- 选择一项资产并按**Ctrl+D**。

### 移动资产
通过以下操作之一移动资产：

- **右键单击**资产，弹出右键菜单，选择**Move To**。
- **拖放**资产到您选择的位置。

![Asset Browser Asset Management.](/images/user-guide/assetbrowser/asset-management.png)

## 导航目录

在Asset Browser中，可以使用**面包屑导航栏**来导航目录，该导航栏会显示当前选定的目录。

面包屑导航栏包括以下实用功能：

- 前 {{< icon "arrow_left-default.svg" >}} 和后 {{< icon "arrow_right-default.svg" >}} 导航箭头可浏览最近选定的目录。
- 用于浏览父目录的 “面包屑”。

    ![Asset Browser Click Navigate.](/images/user-guide/assetbrowser/breadcrumbs-click-navigate.png)

- 用于指定文件位置的地址栏

    ![Asset Browser Edit.](/images/user-guide/assetbrowser/breadcrumbs-edit.png)

## 打开资产

在Asset Browser中，您可以通过**双击**资产来打开和编辑资产。

- 要在编辑器中打开资产，可将资产从 “Asset Browser ”拖放到[Entity Outliner](/docs/user-guide/editor/entity-outliner/)或[视口](/docs/user-guide/editor/viewport/)。
- 要使用关联应用程序打开资产，请**右键单击**资产打开右键菜单，然后选择**Open with associated application...**。
- 要在文件资源管理器中查看资产，**右键单击**打开上下文菜单，然后选择**Open in Explorer**。

## 搜索和筛选资产

对于拥有许多资产的项目，搜索和筛选所需的资产非常有用。在搜索过滤框中输入文字，即可找到特定资产。

### 搜索字段和类型筛选器

要在Asset Browser中搜索和筛选资产，请执行以下操作之一：
- 在搜索字段中输入资产名称。

- 点击过滤器图标并执行以下操作之一，按**资产类型**进行筛选：

  - 在过滤器的搜索栏中输入资产类型
  - 在筛选器选项中滚动，选择所需的资产类型。

  所选资产类型的任何资产都会出现在搜索结果中。

- 选择**Clear**，清除搜索结果。

在下面的示例图片中，会出现 **材质** 或 **物理材质** 资产。

![Asset Browser Asset Type Filter.](/images/user-guide/assetbrowser/asset-type-filter.png)

### 高级资产过滤选项

1. 在**Asset Browser**中，打开**Asset Browser Menu**

    ![Asset Browser Advanced Filter Options.](/images/user-guide/assetbrowser/advanced-filter-options.png)

2. 根据需要使用以下选项：

  - 禁用**Hide Engine Folders**，以显示不在项目文件夹内的资产。

    必须禁用此选项才能查看您可能添加的任何外部目录，包括 **Asset Gems**。

   - 禁用**Hide Unusable Product Assets**以显示编辑器不能使用的资产。

        您可以使用[设置注册系统](/docs/user-guide/settings)，通过创建带有`.setreg`扩展名的 JSON 来添加或删除**不可用产品资产**。

     在新的 JSON 文件中，指定所需的**产品资产**及其**UUID**，并使用以下 JSON 路径： `/O3DE/AssetBrowser/AssetTypeUuidExcludes`. 
        
        **示例**

        ```json
        {
        "O3DE": {
            "AssetBrowser": {
                "AssetTypeUuidExcludes": {
                    "AzslOutcomeAsset":  "{6977AEB1-17AD-4992-957B-23BB2E85B18B}",
                    "ModelLodAsset": "{65B5A801-B9B9-4160-9CB4-D40DAA50B15C}",
                    "ImageMipChainAsset": "{CB403C8A-6982-4C9F-8090-78C9C36FBEDB}", 
                    "BufferAsset": "{F6C5EA8A-1DB3-456E-B970-B6E2AB262AED}", 
                    "ShaderVariantAsset": "{51BED815-36D8-410E-90F0-1FA9FF765FBA}"
                                        }
                            }
                }
        }
        ```
