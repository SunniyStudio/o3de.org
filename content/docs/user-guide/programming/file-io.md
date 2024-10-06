---
description: ' 直接访问文件，用于特殊用途，如在 Open 3D Engine 中进行流式传输。 '
title: O3DE 中的原始文件访问
---

本主题描述了如何在特殊情况下直接访问 O3DE 中的文件。然而，我们建议您使用 O3DE 资产系统来处理资产文件。在大多数情况下，不需要原始文件访问。更多信息请参阅 [使用资产管道和资产文件](/docs/user-guide/assets/)。

当您编写一个`AssetHandler`派生类在 O3DE 中加载资产时，运行时文件处理是自动的。然而，某些情况下可能需要在运行时进行较低级别的文件访问。可能需要低级文件访问的情况包括：
+ 在 `.pak` 文件加载和可用之前，在启动过程中从部署根目录加载原始配置文件。
+ 直接访问 `.pak` 文件中的文件。
+ 直接访问磁盘上任意位置的文件。
+ 流文件格式（如音频或视频），不加载整个文件，而是回放文件。这种方法通常使用中间件来回放或捕获音频、视频或顶点数据。大多数此类系统都需要你实现文件钩子来执行`read`, `seek`, `tell`, `open`, 和 `close`等操作。在这些情况下，直接访问文件可能比将文件视为资产并使用 `AZ::Data`系统对其执行操作更容易。
+ 其他需要直接访问文件的遗留系统或中间件，而这些系统无法使用`AZ::DataAsset`系统。但请注意，可以编写文件访问临时文件，说明如何为大多数中间件访问文件。
+ 从资产缓存以外的位置加载原始源文件。从资产缓存以外的位置加载源文件只适用于 O3DE 编辑器内的工具。只有资产缓存会随游戏一起发送，因此无法在运行时从资产缓存以外的位置加载原始源文件。

`AZ::IO`命名空间中的少量类（如 `FileIOBase` 和 `FileIOStream.` 等）涵盖了需要直接处理文件的少数情况。

## FileIOBase虚类

纯虚基类`FileIOBase`\(位于`\dev\Code\Framework\AzCore\AzCore\IO AzCore\IO\FileIO.h`\) 定义了访问文件的 API。正如下面的代码所示，这是一个基本的阻塞式低级文件 API：

```cpp
class FileIOBase
{
...
    static FileIOBase* GetInstance();  ///< Use this to get a concrete instance of the API that follows.
...
    virtual Result Open(const char* filePath, OpenMode mode, HandleType& fileHandle) = 0;
    virtual Result Close(HandleType fileHandle) = 0;
    virtual Result Tell(HandleType fileHandle, AZ::u64& offset) = 0;
    virtual Result Seek(HandleType fileHandle, AZ::s64 offset, SeekType type) = 0;
    virtual Result Read(HandleType fileHandle, void* buffer, AZ::u64 size, bool failOnFewerThanSizeBytesRead = false, AZ::u64* bytesRead = nullptr) = 0;
    virtual Result Write(HandleType fileHandle, const void* buffer, AZ::u64 size, AZ::u64* bytesWritten = nullptr) = 0;
    virtual Result Flush(HandleType fileHandle) = 0;
    virtual bool Eof(HandleType fileHandle) = 0;
...
};
```

`FileIOBase`类包含查找文件、创建或删除目录以及检查属性的操作。此外，该类还包含一个目录别名系统，本文档稍后将介绍该系统。

### 获取I/O接口的实例 

几乎所有的文件操作都需要调用 `GetInstance` 函数来获取文件 I/O 接口的实例：

```
AZ::IO::FileIOBase::GetInstance()
```

### 注意 
+ `FileIOBase` 类中的所有文件操作都是阻塞式的。
+ `FileIOBase` 文件操作的行为类似于 C 语言 API 中的 `fopen`、`fread` 和 `fclose` 操作，但具有 64 位感知，并可处理超大文件。
+ 由于 `FileIOBase` 实例是在引擎初始化时创建和初始化的，因此它通常总是可用的。它可以检查 `.pak` 文件和磁盘上的任意文件。更多信息，请参阅本文档后面的 [FileIO Stack](#file-access-direct-fileio-stack) 。

    {{< note >}}
  由于 `.pak` 文件只有在应用程序启动后才会被初始化，因此在挂载之前尝试访问 `.pak` 文件中的数据将失败。
{{< /note >}}

有关详细信息，请参阅 `FileIO.h` 文件中的代码注释。

## 别名系统

除了上面提到的一系列文件函数外，`FileIOBase`类还提供**目录别名**。目录别名是添加到文件名中的前缀。别名表示`.pak`文件的虚拟目录或磁盘上的任意位置。

{{< note >}}
我们建议您始终使用别名系统来引用缓存中的文件。切勿使用绝对路径。缓存中的文件可能位于“.pak ”文件中或移动设备上的意外位置。在这种情况下，使用绝对路径名可能会失败。
{{< /note >}}

### 获取别名的路径

要获取与别名相关的路径，请使用 `GetAlias` 函数。

```
FileIOBase::GetAlias()
```

### 别名清单

本节介绍目录别名的使用。

**`@assets@`**
指资产缓存目录。未指定别名时，这是默认别名。请注意以下几点：
+ 由于 `@assets@` 是默认的别名，代码可以简单地通过名称加载文件（例如 `textures\MyTexture.dds`），而无需使用资产系统。这样就无需在代码中使用 `@assets@` 别名。

    {{< note >}}
  如果要从资产缓存中加载文件，请不要在文件名前加上`@assets@`别名。只有在必须更改默认行为时，才需要使用别名。这种最佳做法可使代码更易于阅读，并增强兼容性。
{{< /note >}}

+ 在 PC 上开发期间，`@assets@` 指向`Cache\<project_name>\pc\<project_name>`目录。发布后，它将指向存放`.pak`文件的目录（而不是存放配置文件的缓存根目录）。
+ 由于资产处理操作可能会锁定资产缓存，因此尝试向资产缓存写入文件可能会导致断言失败。请勿尝试将文件写入资产缓存。

**`@root@`**
指定项目根配置文件的位置。请注意以下几点：
+ 资产缓存 `@assets@` 可以是 `@root@` 的子目录，但并非总是如此。因此，不要作此假设。如果要加载根文件，请使用 `@root@`。如果要加载资产，要么不使用别名（因为 `@assets@` 是默认的），要么使用 `@assets@`。
+ 在开发过程中，`@root@`目录映射到您的`<project_name>`目录。在发行版中，该目录是游戏发行版的根目录。

**`@user@`**
指定在游戏会话之间存储数据的可写目录。请注意以下几点：
+ 预计用户不会删除该目录。
+ 在 PC 上，`@user@` 是 `dev\Cache\<game_name>\pc\user` 目录。
+ 在其他操作系统和设备上，`@user@` 位置可能不同。某些移动操作系统对应用程序写入文件的位置有限制。

**`@cache@`**
指定用于存储临时数据的可写目录。用户可以随时删除这些数据。

**`@log@`**
指定用于存储诊断日志的可写目录。

### 代码示例

A. 下面的代码示例将打开 assets 目录中的文件。

```cpp
using namespace AZ::IO;
HandleType fileHandle = InvalidHandle;
// 因为需要 @assets@\config\myfile.xml ，所以不必指定别名。
// @assets@ 别名中的所有文件都是小写。这消除了对大小写敏感环境的担忧。
if (FileIOBase::GetInstance()->Open("config/myfile.xml", OpenMode::ModeRead|OpenMode::ModeBinary, fileHandle))
{
    // 打开成功。使用 FileIOBase 的其他 API 操作对句柄执行操作。记住要关闭文件！
    FileIOBase::GetInstance()->Close(fileHandle);
}
```

请注意，由于前面的示例中使用了别名，因此即使 `config\myfile.xml` 文件位于 `.pak` 文件中，也会被找到。

B. 下面的代码示例在日志目录中打开一个文件，并向其中添加日志行。

```cpp
using namespace AZ::IO;
HandleType fileHandle = InvalidHandle;
// In this rare case, you want to write to a file in the @log@ alias,
// so the file name must be specified.
// Because you're writing a file to a non-@assets@ directory, it can contain case.
if (FileIOBase::GetInstance()->Open("@log@/gamelog.txt", AOpenMode::ModeAppend|ModeText, fileHandle))
{
    // Open succeeded. Use other API operations of FileIOBase to perform operations with the handle.
    FileIOBase::GetInstance()->Close(fileHandle);
}
```

`AZ::IO`命名空间中的`FileIOStream`类会在文件退出作用域时自动关闭文件，并将其显示为`GenericStream`接口。这为流媒体系统和序列化系统等期望使用通用流的系统提供了兼容性。

### 仅用于工具的别名 

以下别名仅适用于编辑器工具。

**`@devroot@`**
指定存放 `engine.json` 等文件的源代码树根目录。这些文件由资产处理器处理，并部署到 `@root@` 指定的缓存中。

**`@devassets@`**
指定源代码树中游戏项目 assets 目录的位置。该目录包含未编译的源文件，如`.tif`或`.fbx`文件。它不包含压缩的 `.dds` 文件或游戏通常使用的其他资产。请注意以下几点：
+ `@devassets@` 是文件打开对话框的一个很好的起点，该对话框会询问用户在哪里保存新文件。
+ 由于现有文件可能在 gem 中，因此不要将它们保存在 `@devassets@`中。相反，当编辑器打开文件时，让编辑器记住文件的位置。然后让编辑器将文件保存到文件的原始位置。
+ 由于并非所有源文件都位于 `@devassets@`（许多位于 gem 中），因此不要试图通过搜索其位置来查找所有源文件。

## FileIO Stack 

为了满足游戏客户端和工具的需求，需要创建多个 `FileIO` 实例。如下图所示，这些实例形成了一个堆栈，文件请求在其中流动。

![File access in local and remote scenarios](/images/user-guide/programming/file-access-direct-1.png)

**Either/Or**分支的行为取决于是否启用了虚拟文件系统（VFS）功能（图中的`RemoteFileIO`）。VFS 可从 Android 和 iOS 等非 PC 设备远程读取资产，允许实时重新加载资产。否则，资产将需要直接部署到游戏设备上。VFS 默认为禁用。要启用 VFS，请编辑引擎的 `Registry\bootstrap.setreg` 配置文件中的 `remote_filesystem` 条目，如下例所示。

```json
-- remote_filesystem - enable Virtual File System (VFS)
"remote_filesystem": 1
```

由于 VFS 功能处于较低层次，文件访问操作通过网络以透明方式通过资产处理器传输到上层。

要通过其他系统发送文件请求，您可以实现自己版本的 `FileIOBase`（或派生类之一，如 `RemoteFileIO` 或 `LocalFileIO`）。如果用自己的实例替换 `GetInstance` 或 `GetDirectinstance` 返回的实例，那么 `FileIO` 系统就会使用你的层。通过用自己的实例替换实例，可以形成一个附加过滤器堆栈。然后将自己的实例调用到先前安装的实例中。

## 异步流处理 

如果要使用异步后台流，请考虑使用 `AZ::IO::Streamer` 类而不是 `FileIOBase`。`Streamer`类内部使用 `FileIOBase`，但它使用异步语义。要使用 `Streamer` 类，需要向其传递数据和截止时间。`Streamer`类会将数据放入缓冲区，并尽最大努力在指定的截止日期前完成请求。

欲了解更多信息，请参阅`\dev\Code\Framework\AzCore\AzCore\IO\Streamer.h`文件中的代码和注释。
