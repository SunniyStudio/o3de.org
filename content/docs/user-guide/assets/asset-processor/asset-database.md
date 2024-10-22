---
linkTitle: 资产数据库
title: 资产数据库
description: 资产处理器使用资产数据库来跟踪其已完成的工作。
weight: 950
toc: true
---

**资产数据库**是一个 SQLite 数据库，由[**Asset Processor**](docs/user-guide/assets/asset-processor)使用，用于跟踪已完成的工作。编辑器或启动器不使用资产数据库，而是使用**Asset Catalog**来跟踪可用资产的信息。

资产数据库位于项目的[**Asset Cache**](docs/user-guide/assets/pipeline/asset-cache)中的`assetdb.sqlite`。

## 资产数据库概述

资产数据库分为以下八个表格。

### ScanFolders 表

**ScanFolders** 表跟踪为项目注册为[扫描目录](docs/user-guide/assets/pipeline/scan-directories) 文件夹的所有可包含资产的文件夹。该表包括当前机器上指向该扫描文件夹的绝对路径。不同贡献者的路径可能不尽相同，因为每个项目实例的路径都是唯一的。

### Files 表

**文件***表跟踪资产处理器在[扫描目录](docs/user-guide/assets/pipeline/scan-directories) 中找到的所有文件。该表跟踪文件所在的扫描文件夹、文件到扫描文件夹的相对路径、文件的最后修改时间以及文件内容的哈希值。资产处理器使用修改时间和哈希值来确定何时需要重新处理文件。

### BuilderInfo 表

如果没有生成器对文件进行处理，文件就不是资产，因此在此过程中的下一个表是 **BuilderInfo** 表。该表跟踪与 [资产生成器](docs/user-guide/assets/pipeline/asset-builders) 相关的信息，其中包含生成器 ID、Guid 和每个生成器的分析指纹。文件匹配模式不在数据库中跟踪，因为它们是在代码中定义的，不需要在这里跟踪，不过你可以在资产处理器的生成器选项卡中查看它们。资产处理器使用分析指纹了解构建程序的变化，以便重新处理与该构建程序相匹配的所有资产。

### Sources 表

**Sources** 表跟踪[源资产](docs/user-guide/assets/pipeline/source-assets)的扫描文件夹键，以及该扫描文件夹到源资产的相对路径。它还跟踪源资产的 Guid，这是引擎通常引用源资产的方式，也是用于跟踪产品资产的资产 ID 的一部分。

### Jobs 表

**Jobs** 表包含所有已完成的工作。作业与资产生成器相关联。有关作业的更多信息，请参阅[处理作业](docs/user-guide/assets/pipeline/asset-builders#process-job) 主题。

SourcePK 是源表中已处理源资产的源 ID。JobKey 是在 CreateJobs 中为该任务注册的描述性字符串。Platform 是资产的预期部署目标，如 PC 或 Android。BuilderGuid 是作业所用生成器的唯一标识符。Status 是作业的结果代码：2 表示失败，3 表示由于源名称超过最大长度而失败，4 表示已完成，5 表示由于缺少源资产而失败。作业运行关键字是作业创建的顺序。每个作业中出现的错误和警告数量也会在此表中进行跟踪。

### SourceDependency 表

**SourceDependency** 表跟踪 [源依赖项](docs/user-guide/assets/pipeline/asset-dependencies-and-identifiers#source-dependencies)源依赖项是指修改后会导致相关作业重新运行的文件。

### Product 表

**Product**表跟踪这些作业生成的产品资产。JobPK 是创建该产品的作业 ID。子 ID 是用于标识该产品资产的资产 ID 的一部分。完整的资产 ID 是该子 ID 和源资产的 UUID。

### ProductDependencies 表

**ProductDependencies** 表跟踪产品资产之间的关系，用于运行时和打包目的。当一个产品资产引用另一个产品资产时，就会出现 [产品依赖关系](docs/user-guide/assets/pipeline/asset-dependencies-and-identifiers#product-dependencies) 。

## 更新资产处理器和资产数据库

通过更改资产处理器代码与资产数据库交互有两个主要工作流程：[添加新查询](#NewQuery) 和[更改资产数据库](#UpdateDatabase)

### 添加新查询 {#NewQuery}

资产数据库查询定义在在 [AssetDatabaseConnection.cpp](https://github.com/o3de/o3de/blob/development/Code/Framework/AzToolsFramework/AzToolsFramework/AssetDatabase/AssetDatabaseConnection.cpp#L49)中。

#### 1. 为查询创建静态常量字符串
添加新查询的第一步是在[AssetDatabaseConnection.cpp](https://github.com/o3de/o3de/blob/development/Code/Framework/AzToolsFramework/AzToolsFramework/AssetDatabase/AssetDatabaseConnection.cpp#L49)的顶部创建几个静态常量字符串。
1. 添加一个新的静态常量字符串 static const char\* ，其中包含查询的名称。按照惯例，这个字符串被命名为QUERY_\*。这些字符通常以名称命名。
   * 示例: `static const char* QUERY_SOURCE_BY_SOURCENAME_SCANFOLDERID = "AzToolsFramework::AssetDatabase::QuerySourceBySourceNameScanFolderID";`
1. 一个静态常量，包含查询的实际 sql 数据。按照惯例，它被命名为 QUERY_\*_STATEMENT。这只是一个标准的 SQLite 查询字符串。参数总是使用参数绑定提供，以避免转义问题。
   * 示例: `static const char* QUERY_SOURCE_BY_SOURCENAME_SCANFOLDERID_STATEMENT = SELECT * FROM Sources WHERE SourceName = :sourcename AND ScanFolderPK = :scanfolderid;";`
   * 通常情况下，查询只能从一个表中返回数据。 这并不妨碍从多个表中返回数据，只是可能需要更多的设置。
   * 查询应返回整行数据。 数据会被送入一个 C++ 结构体，该结构体希望每一列都存在，因此不要过滤列。仅返回单个字段的查询是个例外，这是允许的。
1. 静态常量查询对象。这是一个将参数和查询绑定在一起的辅助类，有助于保持类型安全。该类使用 MakeSqlQuery 函数创建。每个查询参数都是使用 SqlParam 类作为参数添加到函数中的，其模板类型与传给构造函数的列类型和参数名称相匹配。
   * 示例:
```cpp
static const auto s_querySourceBySourcenameScanfolderid = MakeSqlQuery(
    QUERY_SOURCE_BY_SOURCENAME_SCANFOLDERID,
    QUERY_SOURCE_BY_SOURCENAME_SCANFOLDERID_STATEMENT,
    LOG_NAME,
    SqlParam<const char*>(":sourcename"),
    SqlParam<AZ::s64>(":scanfolderid"));
```

#### 2. 注册查询对象
1. 下一步是向系统注册查询对象。查询对象已在上一节的步骤 1.3 中定义。大多数查询按表分组。只需调用 AddStatement。
   * 示例:
```cpp
AddStatement(m_databaseConnection, s_querySourceBySourcenameScanfolderid);
```

#### 3. 添加查询功能
1. 添加查询函数，使查询可用。
   * 这些函数通常按表分组，并包含一组查询参数和一个处理程序。处理程序是一个函数，它接收一个代表表中记录的对象，并返回一个 bool，表示查询是否应继续到下一行。
   * 每个现有表都有预制的处理程序定义，还有一个用于查询所有source/job/product/scanfolder表数据的 combinedHandler。在大多数情况下，这些处理程序应该非常简单，因为已经为典型用例提供了结果处理程序函数。
   * 示例:
```cpp
bool AssetDatabaseConnection::QuerySourceBySourceNameScanFolderID(const char* exactSourceName, AZ::s64 scanFolderID, sourceHandler handler)
{
    return s_querySourceBySourcenameScanfolderid.BindAndQuery(*m_databaseConnection, handler, &GetSourceResult, exactSourceName, scanFolderID);
}
```

#### 4. 可选 - 辅助获取功能
1. 添加一个辅助获取函数，返回结果行的容器。[AssetDatabase.cpp](https://github.com/o3de/o3de/blob/development/Code/Tools/AssetProcessor/native/AssetDatabase/AssetDatabase.cpp) 包含用于检索整个数据集的辅助函数，无需每次都编写处理程序。大多数查询都在该类中添加了辅助版本。
   * 示例:
```cpp
bool AssetDatabaseConnection::GetSourcesBySourceNameScanFolderId(QString exactSourceName, AZ::s64 scanFolderID, SourceDatabaseEntryContainer& container)
    {
    bool found = false;
    bool succeeded = QuerySourceBySourceNameScanFolderID(exactSourceName.toUtf8().constData(),
        scanFolderID,
        [&](SourceDatabaseEntry& source)
    {
        found = true;
        container.push_back();
        container.back() = AZStd::move(source);
        return true;  // return true to continue iterating over additional results, we are populating a container
    });
    return  found && succeeded;
}
```

### 更新数据库结构 {#UpdateDatabase}
{{< important >}}
资产数据库结构的更改只能在仔细配置后进行。

在修改现有列时，要牢记对使用现有数据库的用户会产生什么影响。 注意可能出现的数据丢失，并在必要时进行处理。
{{< /important >}}

{{< note >}}
SQLite ALTER TABLE 语法不支持删除现有列。您可能需要在现有数据库中保留该列，删除整个表并处理随之而来的数据丢失，或将现有数据迁移到没有该列的新表中。
{{< /note >}}

#### 1. 更新资产数据库版本

在[AssetDatabaseConnection.h,](https://github.com/o3de/o3de/blob/development/Code/Framework/AzToolsFramework/AzToolsFramework/AssetDatabase/AssetDatabaseConnection.h#L70)中添加一个新条目，描述您的版本变化。

#### 2. 更新相关表的结构定义

在 [AssetDatabaseConnection.h,](https://github.com/o3de/o3de/blob/development/Code/Framework/AzToolsFramework/AzToolsFramework/AssetDatabase/AssetDatabaseConnection.h#L93)中更新表示您计划修改的表的结构，以匹配您的修改。

#### 3. 更新相关表的结构实现

在 [AssetDatabaseConnection.cpp,](https://github.com/o3de/o3de/blob/development/Code/Framework/AzToolsFramework/AzToolsFramework/AssetDatabase/AssetDatabaseConnection.cpp#L999) 中，更新相关结构函数的实现，以实现所做的更改。

#### 4. 更新表创建查询

在 [AssetDatabase.cpp,](https://github.com/o3de/o3de/blob/development/Code/Tools/AssetProcessor/native/AssetDatabase/AssetDatabase.cpp#L32) 中，根据所实施的更改更新表创建查询。

#### 5. 更新现有表格查询

在[AssetDatabase.cpp,](https://github.com/o3de/o3de/blob/development/Code/Tools/AssetProcessor/native/AssetDatabase/AssetDatabase.cpp#L411)中更新现有的表查询。查找名称与 `INSERT_*_STATEMENT` 和 `UPDATE_*_STATEMENT` 相匹配的常量，用要更改的相关表替换星号。

这些查询通常是组合在一起的，但在搜索该文件中的查询时一定要彻底，因为遗漏查询可能会导致数据丢失或损坏。

#### 6. 创建新的更新查询

在[AssetDatabase.cpp,](https://github.com/o3de/o3de/blob/development/Code/Tools/AssetProcessor/native/AssetDatabase/AssetDatabase.cpp#L781)中添加新的升级语句，以处理修改现有数据库的问题。有关现有升级查询的示例，请搜索 `ALTER TABLE`。

#### 7. 在 PostOpenDatabase 中添加更新语句

在 [AssetDatabase.cpp,](https://github.com/o3de/o3de/blob/development/Code/Tools/AssetProcessor/native/AssetDatabase/AssetDatabase.cpp#L831)中滚动到 `AssetDatabaseConnection::PostOpenDatabase()` 函数定义的底部，找到最后一条升级语句，并添加一条新语句。

升级块应检查上次升级块的版本，执行您的语句，将 foundVersion 更新为您的版本，并打印出新版本。

示例:
```cpp
if(foundVersion == AssetDatabase::DatabaseVersion::AddedSourceDependencySubIdsAndProductHashes)
{
    if(m_databaseConnection->ExecuteOneOffStatement(INSERT_COLUMN_PRODUCTS_FLAGS))
    {
        foundVersion = AssetDatabase::DatabaseVersion::AddedFlagsColumnToProductTable;
        AZ_TracePrintf(AssetProcessor::ConsoleChannel, "Upgraded Asset Database to version %i (AddedFlagsColumnToProductTable)\n", foundVersion);
    }
}
```

#### 8. 注册升级查询

在[AssetDatabase.cpp](https://github.com/o3de/o3de/blob/development/Code/Tools/AssetProcessor/native/AssetDatabase/AssetDatabase.cpp#L831)中的 AssetDatabaseConnection::CreateStatements 函数中注册升级查询。

示例:
```cpp
m_databaseConnection->AddStatement(INSERT_COLUMN_PRODUCTS_FLAGS, INSERT_COLUMN_PRODUCTS_FLAGS_STATEMENT);
```

#### 9. 更新现有绑定调用

在[AssetDatabase.cpp,](https://github.com/o3de/o3de/blob/development/Code/Tools/AssetProcessor/native/AssetDatabase/AssetDatabase.cpp#L2300) 中更新现有的绑定调用。

现有绑定调用示例，更新时应注意的事项：
```cpp
if (!s_UpdateProductQuery.Bind(*m_databaseConnection, autoFinalizer, entry.m_jobPK, entry.m_subID, entry.m_productName.c_str(), entry.m_assetType, entry.m_legacyGuid, entry.m_flags.to_ullong(), entry.m_productID, entry.m_hash))
```

现有的绑定调用可能存在于该文件的多个位置，但一般来说，未更新的绑定调用会出现编译错误。

#### 10. 测试并验证您的变更

除了您可能希望执行的手动测试以验证您的更改外，我们还建议您编写自动测试。

资产处理器的 C++ 自动测试可在 [此处](https://github.com/o3de/o3de/tree/development/Code/Tools/AssetProcessor/native/tests)找到，AzToolsFramework 的自动测试可在[此处](https://github.com/o3de/o3de/tree/development/Code/Framework/AzToolsFramework/Tests)找到。

资产处理器的 Python 自动测试可在自动测试项目中找到，[此处](https://github.com/o3de/o3de/tree/development/AutomatedTesting/Gem/PythonTests/assetpipeline/asset_processor_tests)。



