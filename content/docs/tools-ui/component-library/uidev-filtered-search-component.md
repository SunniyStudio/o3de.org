---
linktitle: 筛选搜索
title: O3DE UI 筛选的搜索 Widget
description: 了解如何使用 O3DE UI 筛选的搜索小部件在您的 O3DE 工具和 Gem 中为用户提供高级搜索选项。
toc: true
---

使用 **筛选的搜索** 小组件在 O3DE UI 工具中为用户提供高级搜索选项。您可以在 O3DE 工具（如 **Entity Outliner** 和 **Asset Browser**）中看到此界面的实际效果。要快速将搜索范围缩小到他们正在寻找的结果，用户可以选择一个或多个“类型过滤器”以应用于他们在搜索字段中键入的搜索词。

## 过滤搜索小部件剖析

O3DE 编辑器中的 Entity Outliner 在筛选的搜索小部件中使用多个筛选条件类别，以帮助用户查找具有特定组件或设置的实体。在以下示例中，我们将查找具有 Script Canvas 组件的所有实体。

![component filtered search anatomy](/images/tools-ui/component-filtered-search-anatomy.png)

1.  **Search field**

    用户在此处输入搜索词。在 **Entity Outliner** 中，他们可以输入实体名称。例如，在上图中，他们可能会输入术语 `Light` 来开始查找名称中包含 `Light` 一词的所有实体。

1.  **Filter menu button**

    （可选）将一个或多个类型筛选器添加到筛选的搜索小组件中，以显示筛选器菜单按钮。当用户选择此按钮时，将打开筛选器菜单。

1.  **Filter type search field**

    用户可以在此字段中输入文本，以缩小筛选器菜单中显示的筛选器类型列表。

1.  **Filter type category**

    使用`AddTypeFilter`函数添加过滤器类型时，您可以指定用于对过滤器类型进行分组的类别。

1.  **Filter type**

    用户可以选择一个或多个筛选条件类型来缩小搜索结果范围，以仅显示与从筛选菜单中选择的类型匹配的结果。您可以使用 `AddTypeFilter` 函数添加过滤器类型以及过滤器类型类别。

1.  **Applied filters**

    默认情况下，用户从筛选器菜单中选择的任何筛选器都将显示在搜索字段下。您可以通过在过滤的搜索小部件上调用`setEnabledFiltersVisible(false)` 来关闭此功能。

    您还可以在应用的过滤器名称前面添加图标。请参阅 [filtered type icon](#filtered-search-with-filter-type-icons) 示例中如何执行此操作。

1.  **Search results**

    在搜索字段下方显示搜索结果。

## 基本筛选搜索

![component filtered search basic](/images/tools-ui/component-filtered-search-basic.png)

以下示例演示如何创建简单的筛选搜索 Widget。

### 示例

```cpp
#include <AzQtComponents/Components/FilteredSearchWidget.h>

// Create a filtered search widget.
auto filteredSearchWidget = new AzQtComponents::FilteredSearchWidget(parent);

// Add a filter type category and filter types.
const QStringList filterTypes = { "Apple", "Orange", "Pear", "Banana" };
const QString filterCategory = "Fruit";
for (const auto& filterType : filterTypes)
{
    filteredSearchWidget->AddTypeFilter(filterCategory, filterType);
}

// Connect the TextFilterChanged signal to a QSortFilterProxyModel.
MyProxyModel proxyModel;
connect(filteredSearchWidget, &AzQtComponents::FilteredSearchWidget::TextFilterChanged,
        proxyModel, static_cast<void (QSortFilterProxyModel::*)(const QString&)>(&MyProxyModel::setFilterRegExp));

// Connect the TypeFilterChanged signal to a proxy model that has a slot that can
// handle changes of the type:
// FilteredSearchWidget::TypeFilterChanged(const SearchTypeFilterList& activeTypeFilters)
connect(filteredSearchWidget, &AzQtComponents::FilteredSearchWidget::TypeFilterChanged,
        proxyModel, &MyProxyModel::ApplyTypeFilters);
```

## 使用筛选器类型图标进行筛选搜索

![component filtered search filter type icons](/images/tools-ui/component-filtered-search-filter-type-icons.png)

使用`AzQtComponents::SearchTypeFilter`的`extraIconFilename` 属性将可选图标添加到筛选器类型。

### 示例

```cpp
#include <AzQtComponents/Components/FilteredSearchWidget.h>

// Create a filtered search widget.
auto filteredSearchWidget = new AzQtComponents::FilteredSearchWidget(parent);

// Add a filter type category and filter types.
const QStringList filterTypes = { "Apple", "Orange", "Pear", "Banana" };
const QString filterCategory = "Fruit";
for (const auto& filterType : filterTypes)
{
    AzQtComponents::SearchTypeFilter filterWithIcon(filterCategory, filterType);
    filterWithIcon.extraIconFilename = ":/stylesheet/img/tag_visibility_on.svg";
    filteredSearchWidget->AddTypeFilter(filterWithIcon);
}
```

## 限制搜索字段的宽度

![component filtered search width](/images/tools-ui/component-filtered-search-width.png)

使用`setTextFilterFillsWidth(false)`来限制搜索字段的宽度，并防止其扩展到小部件的整个宽度。

### Example

```cpp
filteredSearchWidget->setTextFilterFillsWidth(false);
```

## C++ API 参考

有关 **筛选搜索** API 的详细信息，请参阅 [O3DE UI 扩展 C++ API 参考](/docs/api/frameworks/azqtcomponents/namespace_az_qt_components.html) 中的以下主题：
+  [AzQtComponents::FilteredSearchWidget](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_filtered_search_widget.html)
