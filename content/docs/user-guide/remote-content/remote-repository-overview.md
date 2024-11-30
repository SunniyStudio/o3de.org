---
linkTitle: 远程存储库概述
title: O3DE远程存储库概述
description: 了解 O3DE 远程存储库。
weight: 600
toc: true
---

远程存储库是`repo.json`文件，其中包含有关可通过 Git 下载或克隆的 Gem、项目和模板的信息，以及用户可能需要了解的其他远程存储库的信息，以便下载 Gem 依赖项，或者只是为了访问其他远程存储库中的相关内容。`repo.json`文件可以位于本地计算机、Web 服务器或 Git 存储库中，例如 [GitHub](https://github.com) 中托管的存储库，可下载内容也可以位于这些位置中的任何一个。 通常 `repo.json` 文件与提供的内容位于同一位置，但并非必须如此。

## O3DE 远程存储库剖析 

O3DE 远程存储库包含一个`repo.json`文件，其中包含存储库的元数据，包括它提供的 Gem、Projects 和 Templates 的 JSON 数据。

### 文件夹结构

#### 单个 Gem、Project 或 Template 远程仓库

如果您只打算在存储库中包含一个 Gem、Project 或 Template，我们建议您将 'repo.json' 文件放在与 `gem.json`,`project.json` 或 `template.json`相同的位置。

在 GitHub 上存储了单个`example.fbx`资产的 Gem 远程存储库示例：
```
/
    Assets/
        example.fbx
    Registry/
        assetprocessor_settings.setreg
    CMakeLists.txt
    gem.json
    preview.png
    repo.json
```

#### 多个 Gem、Projects 或 Templates 远程存储库

如果您打算在远程存储库中拥有多个 Gem、Projects 或 Templates，有两种方法：

#### 1. 使用 `Gems`, `Projects`, 和 `Templates` 子文件夹 

建议的文件夹结构是将`repo.json`文件放在 O3DE 远程存储库的根目录下，将 Gem、Projects 和 Templates 放在相应的“Gem”、“Projects”和“Templates”子文件夹中
```
/
    repo.json
    Gems/
        ExampleGem1/
            gem.json
        ExampleGem2/
            gem.json
    Projects/
        ExampleProject1/
            project.json
    Templates/
        ExampleTemplate1/
            template.json
```
这种方法对于通常提交影响多个 Gem、Projects 和模板的更改的贡献者非常有用，但这意味着没有一种简单的方法可以只使用 Git 获取单个 Gem、Project 或 Template，而不使用像[`git sparse-checkout`](https://git-scm.com/docs/git-sparse-checkout)。

#### 2. 使用 Git 分支

另一种方法是使用 git branches，以便每个远程对象都位于唯一的分支中。 当您想使用 Git 克隆存储库而不是下载存档时，这可能很方便。 这也意味着您可以在 Git 存储库中存储许多对象，当用户克隆它时，他们可以指定他们想要的分支，并且只能获得一个对象。这种方法的缺点是，贡献者必须对影响多个对象的更改进行多次提交，因为它们位于自己的分支中。

1. 将 `repo.json` 放在 `main` 分支中，与存储库的 `README.md` 并列。
1. 为每个 Gem、Project 和 Template 创建一个 Git 分支，以便每个分支在存储库的根目录中只有一个对象。

具有 2 个 Gem 的远程存储库示例：

`main` 分支内容
```
/
    repo.json
    README.md
```

`ExampleAssetGem` 分支内容
```
/
    Assets/
    Registry/
    CMakelists.txt
    gem.json
    preview.png
``` 

`ExampleCodeGem` 分支内容
```
/
    Code/
    Registry/
    .gitignore
    CMakelists.txt
    gem.json
    preview.png
``` 


### repo.json 文件的格式
`repo.json`文件包含 O3DE 远程存储库的元数据。有关`repo.json`架构的更多详细信息，请参阅 [repo.json 参考](repo-json-reference)。

### 与 O3DE 远程存储库相关的 JSON 字段

位于O3DE远程存储库中的 `gem.json`, `project.json` 和 `template.json` 文件应该包含 `version`, `repo_uri`, `last_updated`, `download_source_uri`, `source_control_uri` 和 `source_control_ref` 字段。

| 字段 | 说明 |
| --- | --- |
| download_source_uri | 包含 Gem 存档的`.zip` 文件的 URI。 这是直接下载到 Gem 的`.zip` 存档：即https://github.com/o3de/o3de-extras/releases/download/1.0/Example-2.0.zip  |
| last_updated | 此文件或 Gem 的上次更新日期 `YYYY-MM-DD`, `YYYY-MM-DD HH:MM:SS`, 或 `YYYY-MM-DDTHH:MM:SS` 格式。 |
| repo_uri | O3DE 远程存储库的 URI。|
| sha256 | `download_source_uri`字段指向的 `.zip` 存档的 SHA-256 摘要。  您可以省略此字段进行测试，但应始终在生产中使用它。 |
| source_control_ref | 此 Gem 版本的源代码控制参考。 这可以是 tag、commit hash 或 branch：即`release-1.0.0`, `0462139`, `development` 等。 |
| source_control_uri | 此 Gem 的源存储库的 URI：即 https://github.com/o3de/o3de-extras.  |

{{< note >}}
有关`gem.json` 字段和示例的完整描述，请参阅 [`gem.json` manifest 文档](/docs/user-guide/programming/gems/manifest/).
{{< /note >}}
