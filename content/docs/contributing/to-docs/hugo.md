---
linkTitle: Hugo
title: 使用 Hugo 的 O3DE 文档贡献
description: 学习使用 Hugo，这是 O3DE 文档的静态站点生成器。
toc: true
weight: 600
---

**Open 3D Engine （O3DE）** 文档网站使用 [Hugo](https://gohugo.io/)静态站点生成器，并使用 [Netlify](https://netlify.com) 进行部署。

有关构建说明和先决条件，请参阅 o3de.org [README.md](https://github.com/o3de/o3de.org#readme)，以及 [Git 工作流程](git-workflow.md)了解有关项目工作原理的更多信息。

## 添加新页面

1. 导航到新页面的`/content/docs`下的目录。
2. 为页面创建一个新的 Markdown 文件：
    - 如果页面是文档的新子部分，请为该子部分创建一个目录，然后创建名为`_index.md`的文件。
    - 如果页面正在添加到文档的现有部分，请创建一个具有适当标题的文件。

    在这两种情况下，Markdown 文件名和目录名称都是页面 URL 的一部分。

3. 至少在文件顶部添加以下 front matter：

    ```yaml
    ---
    title: Page title
    toc: true
    ---
    ```
    
    这会将页面添加到主文档菜单，并在每个页面上生成一个目录。

    某些部分使用`weight` frontmatter 属性对页面进行排序，并使用`description` 属性。有关更多信息，请参阅 [Hugo 的文档](https://gohugo.io/content-management/front-matter#front-matter-variables)。

## 添加新指南

只有在维护者批准的情况下，或者如果您自己是维护者，才能添加新指南。

1. 在 `/content/docs` 下直接添加一个新目录，并在该目录中创建一个`_index.md`文件。

2. 添加以下 frontmatter：

    ```yaml
    ---
    linktitle: Title that appears in the left hand navigation
    title: Title that appears at the top of the page
    description: Short description of the page
    weight: Numerical value 
    menu_uuid: example //see below
    guide_img: "/images/example/guide_img.svg" //see below
    ---
    ```

    - `menu_uuid` 字段是指南的简短唯一标识符，没有空格或特殊字符。此字段用于为菜单的折叠面板生成 CSS 类。

    - `guide_img` 字段是`/static`目录中图像的相对 URL，该图像在 Learn 登录页面上用作指南的图像。

## 添加重定向

如果您移动或重命名页面，最好添加从旧页面到新页面的重定向。这是通过 `/static/_redirects` 文件完成的。此文件映射到标准 Apache 重定向文件 （.htaccess），并使用相同的语法来添加重定向。

要添加重定向，请在  `_redirects` 文件中添加一行：

```
/old_url/partial/      /new_url/partial

```


## 添加顶部导航菜单项

通过网站存储库根目录中的 `config.toml` 文件将新项目添加到顶部导航：

```toml
[[menu.main]] // menu.main is the main navigation menu
name = "Learn" //name of the menu item 
url = "#" // relative URL – for example, `/docs` links to the docs homepage
weight = 4 // menu ordering
identifier = "learn" // Identifier used for building menus with submenu items
```

有关更多信息，请参阅 [Hugo 的菜单文档](https://gohugo.io/content-management/menus/).

### 菜单子项

如果你想让菜单包含子菜单项的下拉菜单，请将下拉菜单的标签定义为菜单项，然后使用`parent`字段定义其子项，以指向它们应该出现在下的项：

```toml
[[menu.main]]
name = "Community"
url = "#"
weight = 3 // menu ordering for the top nav itself
identifier = "community" //matches the parent fields below

[[menu.main]]
name = "Community subitem"
url = "#"
weight = 1 // menu ordering for items in the dropdown
parent = "community" //matches the identifier above

[[menu.main]]
name = "Community subitem 2"
url = "#"
weight = 2 // menu ordering for items in the dropdown
parent = "community" //matches the identifier above
```

## 添加或更新社交媒体帐户

社交媒体帐户作为数据存储在`config.toml`文件中，并通过循环读入布局。您可以添加任何可以找到图标的社交媒体帐户！

```toml
[[params.social]]
name = "Discord" // The name of the social media account
url = "#" // The URL to point to 
icon = "fas fa-envelope" // the FontAwesome Icon classes 
```

社交媒体帐户的图标是 [FontAwesome Icons](https://fontawesome.com/icons?d=gallery&p=2)。社交媒体参数存储用于从 FontAwesome 库获取图标的 CSS 类。要找到这些：

1. 导航到特定图标的 FontAwesome 页面。例如，[Here's Discord](https://fontawesome.com/icons/discord?style=brands)。

2. 找到为图标截取的 HTML。在本例中，如下所示：

    ```html
    <i class="fab fa-discord"></i>
    ```

3. 提取两个类 - 例如， `fab fa-discord`. 

4. 将这些添加到`icon` 字段中。

## 更新顶级页面

O3DE 的顶级页面 – `/docs`, `/download` 和 `/community` – 都使用自定义布局。您需要 HTML 和 CSS 的基本知识（以及 [Bootstrap](https://getbootstrap.com/)） 来更新这些页面的外观。

在所有情况下，以下文件都是关键：

* `/assets/sass/custom.sass`: 存储布局的所有自定义 SCSS。
* `/assets/js/app.js`: 存储页面的 Javascript。如果您需要更新下载页面，这一点很重要，因为下载页面使用 Javascript 来解析浏览器的用户代理。
*  `/layouts/partials/javascript.html`: 将 Javascript 文件添加到页面。如果要向站点添加新脚本，则为 Important。

### 更新顶级页面内容

每个页面的正文内容（自由格式文本）存储在页面的`/content/.../_index.md` 文件中。这是常规的 markdown 内容，可以随意更新。

### 更新顶级页面布局结构

本节是对 Hugo 的布局/模板引擎的非常简短的概述。有关更多信息，请参阅 [Hugo 的模板文档](https://gohugo.io/templates/introduction/)。

#### 了解 Hugo 布局结构

Hugo 布局遵循 [查找顺序](https://gohugo.io/templates/lookup-order/)。`/layouts/_default` 目录包含基本布局，如果没有更具体的可用，Hugo 会渲染它。如果 Hugo 找到一个布局目录（例如`/layouts/download` ），它与内容目录`/layouts/download` 匹配相同的文件路径，则会将布局应用于该内容页面及其子页面。如果找不到，它会应用 `/layouts/_default` 中的布局。

在布局目录中，Hugo 寻找以下 4 种基本类型之一：

* `baseof.html`: “base” 模板。除非您知道自己在做什么，否则不要覆盖此内容！
* `section.html`: `_index.md`  文件或部分主页的模板。
* `single.html`: 目录中 _not_ `_index.md` 文件的模板 – 即常规页面。
* `list.html`: 子页面列表的模板。由于这与 `section.html` 有点混淆，因此请仅将此布局用于博客部分。

O3DE 网站中的大多数顶级页面都是没有子页面的“section.html”页面。

#### 理解 Hugo 布局继承

如上所述，Hugo 会逐渐寻找更具体的布局，并应用它能找到的最具体的布局。但是，它也通过允许您覆盖名为 `main` 的部分来执行 _within_ 布局文件。

例如，让我们看看`/layouts/_default/baseof.html`:

```html
{{ $lang := site.LanguageCode }}
<!DOCTYPE html>
<html{{ with $lang }} lang="{{ . }}"{{ end }}>
  <head>
    <title>
      {{ block "title" . }}
        {{ site.Title }}
      {{ end }}
    </title>
    {{ partial "css.html" . }}
    {{ partial "favicon.html" . }}
  </head>

    <body>
      {{ partial "navbar.html" . }}
    <main>
      {{ block "main" . }}
      {{ end }}
    </main>
    {{ partial "javascript.html" }}
  </body>
</html>
```

接下来，我们来看一下`/layouts/_default/section.html`:

```html
{{ define "main" }}
  <div class="container">
      <section class="row">
        <section class="col col-8">
          <h1 class="title">{{ .Page.Title | markdownify }}</h1>
          {{ with .Content }}
          <div class="content">
            {{ . }}
          </div>
          {{ end }}
        </section>
      </section>
    </div>
{{ end }}
```

请注意，`section.html`只定义了 `main` 块。在没有任何其他块的情况下，Hugo 希望`baseof.html`来提供页面布局的其余部分。

例如，这在其他子页面上以类似的方式工作`/layouts/download.html`:

```html
{{ define "main" }}
{{ partial "download/hero.html" . }}
{{ with .Content }}
<section class="container-xl mt-5 pt-5">
  <div class="row">
    <div class="col-12">
      {{ . }}
    </div>
  </div>
</section>
{{ end }}   
{{ partial "footer.html" . }}  
{{ end }}

```

在这种情况下，布局执行以下操作：
* 覆盖 `/layouts/_default/section.html` 的 `main` 部分。
* 调用 `/download/hero.html` 部分（有关布局部分的更多信息，请参阅 [Hugo 的部分模板文档](https://gohugo.io/templates/partials/)）
* 为正文内容添加一个 (`{{ with .Content }}`)
* 将 `footer.html` 称为部分

因此，如果您想更新下载页面的工作方式，您可以从 `/layouts/downloads/section.html` 开始。但是，如果您需要在下载页面的  `<head>` 部分添加内容，则需要将`layouts/_default/baseof.html` 复制到 `/layouts/downloads` 目录并在那里覆盖它。
