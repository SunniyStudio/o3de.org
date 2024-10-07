---
linktitle: AzAutoGen 源代码生成器
title: 使用 AzAutoGen 从模板自动生成源代码
description: O3DE 功能可以使用 AzAutoGen 在给定 Jinja2 模板和数据文件的情况下自动生成代码和资产。了解 AzAutoGen 如何工作，如何将 AzAutoGen 集成到您的功能流程中，以及如何创建模板和数据文件。
weight: 200
---

3D Engine (O3DE)** uses **AzAutoGen**, its own lightweight generator system.
要创建许多类似的模板类或资产，最有效的解决方案往往是从模板和数据输入自动生成。在源文件生成方面，**Open 3D Engine (O3DE)** 使用了**AzAutoGen**，这是它自己的轻量级生成系统。

AzAutoGen 是一个使用 [Jinja2](https://jinja.palletsprojects.com/)模板引擎的 Python 工具。给定一个 Jinja 模板和一组 XML 或 JSON 数据，AzAutoGen 就会生成一组输出文件。您自己的 O3DE Gem 和项目可以在构建过程中使用 AzAutoGen 生成输出源文件，如代码或资产。有关在构建过程中调用 AzAutoGen 的信息，请参阅[使用 AzAutoGen 构建生成的源文件](/docs/user-guide/build/generated-source/)。

本主题将解释 AzAutoGen 如何工作，并让您熟悉 JSON 和 XML 输入格式。

## AzAutoGen 的工作原理

AzAutoGen 通过两个输入集工作。您可以授权这些文件，并根据项目所需的功能定义这些规则。
  
- 数据输入文件和模板文件的集合，AzAutoGen 使用这些文件自动生成输出代码或资产。
- [**自动生成规则**](/docs/user-guide/build/generated-source#autogen-rules)列表，用于将数据输入文件映射到模板文件，并定义其输出文件名。


然后，可以通过将 AzAutoGen 集成到 CMake 编译过程中来调用它。有关如何设置集成的详细信息，请参阅 [构建生成的源文件](/docs/user-guide/build/generated-source/)。

AzAutoGen 调用后会执行以下步骤。(您可以在 [`cmake/AzAutoGen.py`](https://github.com/o3de/o3de/blob/5c733c3a34931e48435ef6ee72b7feddeac0e03b/cmake/AzAutoGen.py)中找到代码。)

  1. 剪除所有没有`.xml`、`.json`或`.jinja`扩展名的输入文件。由于会出现这种情况，你可以将模板放在与其他代码相同的位置，但不建议这样做。

  1. 将 `.xml` 和 `.json` 输入文件归类为源文件或数据文件，将 `.jinja` 归类为模板文件。

  1. 对于构成自动生成规则的每一组输入、模板和输出文件名，将相应文件匹配到相应的文件集中。模板文件必须是单个文件。每个输入文件都会通过该模板生成相应的文件，并根据输出文件名中的模式命名。

  1. 对于每个 XML 或 JSON 数据文件：

        1. 数据通过一个单一的 Jinja 模板进行处理。在模板内部，Jinja 可以访问一个本地 Python 对象，该对象代表输入文件的内容。

        1. 输出结果将写入相应的文件。


## 编写 Jinja 模板和数据输入

如何在 Gem 或项目中使用 AzAutoGen，取决于在 Jinja 模板中编写的功能和提供的数据输入。

在使用Jinja模板之前，了解一些Jinja的概念会有所帮助。
一个**Jinja 模板**是一个简单的文本文件，它接收数据，用提供的数据替换模板中的部分内容，并输出最终文件。
模板包含**变量**和**表达式**（数据替换这些变量和表达式），以及**标记**（输出文件应如何显示的逻辑指令）。**数据**是输入模板的值。对于 AzAutoGen，您可以用 XML (`.xml`) 或 JSON (`.json`) 文件编写数据。

Jinja2 的模板系统还可以使用一组数据文件来生成一组输出文件，因此，如果需要生成许多文件，AzAutoGen 是一种高效的解决方案。

有关编写 Jinja 模板和数据输入的更多信息，请参阅 Jinja 网站上的[模板设计器文档](https://jinja.palletsprojects.com/templates/) 。

### 输入映射和变量

作为 Jinja2 模板引擎的一部分，AzAutoGen 可以访问以 Python 对象形式暴露给模板系统的数据。JSON 输入文件直接作为 Python 字典加载，XML 文件则由 Python 的 [xml.etree.ElementTree](https://docs.python.org/3/library/xml.etree.elementtree.html) 对象表示。

在制作 Jinja 模板时，您可以使用以下变量。AzAutoGen 定义这些变量及其属性，设置它们的值，然后通过 Jinja 的模板系统生成输出文件。
| Name | Value |
|-|-|
| `dataFiles` | 包含从输入文件读取的对象的字典数组。 |
| `dataFileNames` | 输入文件名的数组。`dataFiles`和`dataFileNames` 都不保证是有序的，但 `dataFileNames[n]`始终是`dataFiles[n]`中可用对象的来源。 |
| `templateName` | AzAutoGen 当前正在处理的模板文件名称。 |
| `outputFile` | 输出文件的名称。 |
| `filename` | AzAutoGen 当前正在生成的文件的名称。|


### 示例

由于 Python XML 元素对象和字典不能提供严格的一对一映射，因此使用不同的输入格式需要使用不同的模板。O3DE 经常使用 XML 格式生成源代码；例如，当 [创建Script Canvas节点](/docs/user-guide/scripting/script-canvas/programmer-guide/custom-nodes/)。

下面的示例展示了如何使用 AzAutoGen 生成大量类似的 `.h` 文件。该示例是 `AzNetworking` 框架的数据包生成模板的简化版本。请注意，XML 和 JSON 数据在结构上存在差异。这些差异在 XML 输入的示例模板中进行了演示，其中包含注释，说明在处理 JSON 时存在的差异。


#### XML 数据

```xml
<PacketGroup Name="CorePackets" PacketStart="0">
    <Packet Name="InitiateConnectionPacket" Desc="This packet is used to initiate a new connection.">
        <member Type="AzNetworking::UdpPacketEncodingBuffer" Name="handshakeBuffer" />
    </Packet>
    
    <Packet Name="ConnectionHandshakePacket" Desc="This packet is used to negotiate the handshake of a new connection.">
        <member Type="AzNetworking::UdpPacketEncodingBuffer" Name="handshakeBuffer" />
    </Packet>

    <Packet Name="TerminateConnectionPacket" Desc="This packet is used to gracefully terminate an existing connection.">
        <member Type="AzNetworking::DisconnectReason" Name="disconnectReason" Init="AzNetworking::DisconnectReason::None" />
    </Packet>

    <Packet Name="HeartbeatPacket" Desc="This packet is used to keep an established connection alive.">
        <member Type="bool" Name="requestResponse" Init="false" />
    </Packet>

    <Packet Name="FragmentedPacket" Desc="This packet is used to segment a packet that exceeds a connections MTU.">
        <member Type="AzNetworking::SequenceId" Name="unfragmentedSequence" Init="AzNetworking::InvalidSequenceId" />
        <member Type="AzNetworking::SequenceId" Name="fragmentSequence" Init="AzNetworking::InvalidSequenceId" />
        <member Type="uint8_t" Name="chunkIndex" Init="0" />
        <member Type="uint8_t" Name="chunkCount" Init="0" />
        <member Type="AzNetworking::ChunkBuffer" Name="chunkBuffer" />
    </Packet>
</PacketGroup>
```

#### XML 数据的模板

```jinja
{% macro CamelCase(text) %}{{ text[0] | upper }}{{ text[1:] }}{% endmacro %}
{%  for data in dataFiles %} {# namespace generation #}
namespace {{ data.get('Name') }}
{
    enum class PacketType
    {
        START = aznumeric_cast<int32_t>({{ data.get('PacketStart') }})
{% for packet in data %} {# (1) #}
    ,   {{ packet.get('Name') }}
{% endfor %}
    ,   MAX
    };

{% for packet in data %} {# class generation #} {# (1) #}
{% set name = packet.get('Name') %}
{% set type = "PacketType::" + packet.get('Name') %}
    
    class {{ name }} final
        : public AzNetworking::IPacket
    {
    public:
        static constexpr AzNetworking::PacketType Type = aznumeric_cast<AzNetworking::PacketType>({{ type }});

        {{ name }}() = default;
{% if len(packet) | len > 0 %}
        explicit {{ name }}
        (
{% for member in packet %} {# (2) #}
        {% if loop.first %}    {% else %},   {% endif %}{{ member.get('Type') }} {{ member.get('Name') }}
{% endfor %}
        );
{% endif %}
        ~{{ name }}() override = default;

        bool operator ==(const {{ name }}& rhs) const;
        bool operator !=(const {{ name }}& rhs) const;

{% for member in packet %} {# (2) #}
        {% set name = CamelCase(member.get('Name')) %}
        {% set type = member.get('Type') %}
        void Set{{ name }}(const {{ type }}& value);
        const {{ type }}& Get{{ name }}() const;
        {{ type }}& Modify{{ name }}();

{% endfor %}
        AzNetworking::PacketType GetPacketType() const override;
        AZStd::unique_ptr<AzNetworking::IPacket> Clone() const override;
        bool Serialize(AzNetworking::ISerializer& serializer) override;
{% if len(packet) | len > 0 %}

    private:

{% for member in packet %} {# (2) #}
        {{ member.get('Type') }} m_{{ member.get('Name') }}{% if member.get('Init') %} = {{ member.get('Init') }}{% endif %};
{% endfor %}
{% endif %}
    };
{% endfor %} {# class generation #}
}
{% endfor %} {# namespace generation #}
```

#### JSON 数据

```json
{
    "Name": "CorePackets",
    "PacketStart": "0",
    "Packets": [{
        "Name": "InitiateConnectionPacket",
        "Desc": "This packet is used to initiate a new connection.",
        "members": [{
          "Type": "AzNetworking::UdpPacketEncodingBuffer",
          "Name": "handshakeBuffer"
        }]
      },{
        "Name": "ConnectionHandshakePacket",
        "Desc": "This packet is used to negotiate the handshake of a new connection.",
        "members": [{
          "Type": "AzNetworking::UdpPacketEncodingBuffer",
          "Name": "handshakeBuffer"
        }]
      },{
        "Name": "TerminateConnectionPacket",
        "Desc": "This packet is used to gracefully terminate an existing connection.",
        "members": [{
          "Type": "AzNetworking::DisconnectReason",
          "Name": "disconnectReason",
          "Init": "AzNetworking::DisconnectReason::None"
        }]
      },{
        "Name": "HeartbeatPacket",
        "Desc": "This packet is used to keep an established connection alive.",
        "members": [{
          "Type": "bool",
          "Name": "requestResponse",
          "Init": "false"
        }]
      },{
        "Name": "FragmentedPacket",
        "Desc": "This packet is used to segment a packet that exceeds a connections MTU.",
        "members": [{
            "Type": "AzNetworking::SequenceId",
            "Name": "unfragmentedSequence",
            "Init": "AzNetworking::InvalidSequenceId"
          },{
            "Type": "AzNetworking::SequenceId",
            "Name": "fragmentSequence",
            "Init": "AzNetworking::InvalidSequenceId"
          },{
            "Type": "uint8_t",
            "Name": "chunkIndex",
            "Init": "0"
          },{
            "Type": "uint8_t",
            "Name": "chunkCount",
            "Init": "0"
          },{
            "Type": "AzNetworking::ChunkBuffer",
            "Name": "chunkBuffer"
        }]
      }
    ]
}
```

#### 针对 JSON 的模板更改

对于 JSON 模板，不能直接对 XML 元素的子元素使用迭代器。相反，您应该对数组进行遍历。为支持 JSON，您需要以下列方式对前面的 XML 输入模板进行修改，并将其修改到每一行的注释中。请注意，您必须在发生迭代的多个地方应用其中一些更改。

```jinja
{% for packet in data.get('Packets') %} {# (1) #}
{% for member in packet %} {# (2) #}
```

您可以使用 Python 灵活的类型和常用方法（如 `.get()`）来编写更容易支持这两种数据格式的 Jinja 模板，这些方法在 `xml.etree.ElementTree.Element` 类和 `dict` 中都可用。


## 相关主题

| 主题 | 说明 |
|-|-|
| [使用 AzAutoGen 生成源文件](/docs/user-guide/build/generated-source/) | 如何使用 CMake 中的 `ly_add_autogen` 函数从模板生成并编译源代码。 |
| [网络自动数据包](/docs/user-guide/networking/aznetworking/autopackets/) | 如何使用 AzAutoGen 为 `AzNetworking` 框架创建新的数据包类型。 |
| [在Script Canvas中创建自定义节点](/docs/user-guide/scripting/script-canvas/programmer-guide/custom-nodes/) | 如何使用 XML 定义在 Script Canvas 中创建自定义节点，并使用 AzAutoGen 将节点转化为代码。 |
