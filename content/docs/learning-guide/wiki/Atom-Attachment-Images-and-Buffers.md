---
title: "[Atom] 附件图像和缓冲区"
description: ""
toc: false
---

# 概述
如果 GPU 资源（图像、缓冲区）在其生命周期的任何时候都可以写入，则如果此通道中提交的任何绘制项目可以使用此资源，则需要将其声明（使用）为通道的通道附件。这些类型的资源称为附件图像或附件缓冲区。</p><p>有些资源的生命周期只需要在一帧的周期内，我们称之为瞬态资源。对于生命周期长于一帧的其他资源，我们将其称为持久资源。对于瞬态资源，它们已经在 Pass 类中实现，该类可以在帧期间创建它们（拥有的附件），并将它们传递给其他连接通道。对于持久性资源，有多种方法可以创建它们并将其附加到传递。
在本文档中，我们将介绍如何使用临时和持久附件资源。
# 使用附件
在本节中，它说明了如何在框架中使用附件。细节已经通过 Pass 的 PassAttachment 实现。但在添加自定义通道或进行调试时，它可能很有帮助。
要将 gpu 资源用作附件，需要在帧的渲染开始之前，通过 `RHI::FrameGraphAttachmentInterface`的`ImportBuffer()` 或 `ImportImage()` 函数将资源导入到帧的附件数据库中。对于每个使用此附件的 Pass （Scope），它需要使用资源调用 `RHI::FrameGraphInterface's UseAttachment()` 函数。最后一件事，任何使用此资源的着色器资源组都需要将其资源视图绑定到着色器资源组。
导入附件步骤是告诉帧图此资源将用作此帧中的附件。使用附件步骤是告诉帧图，此附件将如何用于此范围。例如，用于读取或写入。
# 瞬态附件
假设在渲染管道中，它需要首先渲染深度图像，并且此深度图像将用作其他通道的输入，例如同一帧中的光剔除通道和前向通道。虽然，此深度图像不需要在此帧之后保留，这使其成为瞬态附件。
对于这样的图像，我们只需要在 pass 模板的 ImageAttachments 数据中声明它。以下数据块是通道模板的 .pass 文件的一部分。使用此通道模板创建的通道将创建一个瞬态图像并将其导入到框架中，并声明将其用于范围。

```JSON
               "ImageAttachments": [
                {
                    "Name": "DepthStencil",
                    "SizeSource": {
                        "Source": {
                            "Pass": "Parent",
                            "Attachment": "SwapChainOutput"
                        }
                    },
                    "ImageDescriptor": {
                        "Format": "D32_FLOAT_S8X24_UINT",
                        "MultisampleState": {
                            "samples": 4
                        },
                        "SharedQueueMask": "Graphics"
                    }
                }
            ],
```
注意：要了解有关 pass 如何处理瞬态资源的更多信息，你可以在 Pass.cpp 文件中搜索 Pass::CreateTransientAttachments。

# 永久附件

与通过传递创建和管理的临时附件不同，用作持久附件的资源通常是手动创建的，然后附加到传递槽，其中一个例外是使用 AttachmentImage 资源。

在 RPI 中，AttachmentImage 类（RPI::Image 的派生类）用于表示用作附件的图像，而 Buffer 类用于表示附件缓冲区。 创建这些资源对象后，可以使用 Pass::AttachBufferToSlot 或 Pass::AttachImageToSlot 函数将它们附加到通道。

资源对象的创建通常发生在功能实现中，例如在功能处理器的初始化中。将这些资源附加到通道必须在通道的 BuildAttachmentsInternal 虚拟函数中进行。

例如，对于具有写入和读取作的眼睛适应功能，需要一个保存历史帧亮度值的缓冲区。由于这些值是在 GPU 上计算和保存的，并且需要跨多个帧保存，因此此缓冲区需要是持久附加缓冲区。

以下块显示了如何创建缓冲区并将其用于通道和相关着色器。
```HLSL
// EyeAdaptaion.azsl, where the buffer is written to
 struct EyeAdaptation
{
	...

    // This is a luminance history buffer. 
    // x = average luminance value by direct metering
    // y = smoothed value using previous buffer value
    float2 m_luminanceHistory[32];
};


ShaderResourceGroup PassSrg : SRG_PerPass
{
    ...
    // Contains the eye-adaptation feedback settings. This buffer will be updated in this pass. 
    RWStructuredBuffer<EyeAdaptation> m_eyeAdaptationData;
}

[numthreads(1, 1, 1)]
void MainCS(uint3 dispatch_id : SV_DispatchThreadID)
{
    ...
	PassSrg::m_eyeAdaptationData[0].m_luminanceHistory[currentBufferIndex].y = smoothedPhysicalLuminance;
	...
}
```
```jsonc
// EyeAdaption.pass pass template where define the input/output slot to attach this buffer to
		"Slots": [
			...
			,
            {
            	"Name": "EyeAdaptationDataInputOutput",
                "SlotType": "InputOutput",
                "ScopeAttachmentUsage": "Shader"
            }
        ],

```
```Cpp
// EyeAdaptationPass.cpp, the customized pass for EyeAdaptation which creates the buffer and attach the buffer
void EyeAdaptationPass::InitBuffer()
{
	AZStd::string bufferName = AZStd::string::format("%s_%p", EyeAdaptationHistoryBufferBaseName, this);

	ExposureCalculationData defaultData;
	RPI::CommonBufferDescriptor desc;
	desc.m_poolType = RPI::CommonBufferPoolType::ReadWrite;
    desc.m_bufferName = bufferName;
    desc.m_byteCount = sizeof(ExposureCalculationData);
    desc.m_elementSize = aznumeric_cast<uint32_t>(desc.m_byteCount);
    desc.m_bufferData = &defaultData;

    m_buffer = RPI::BufferSystemInterface::Get()->CreateBufferFromCommonPool(desc);
}


void EyeAdaptationPass::BuildAttachmentsInternal()
{
	...
	if (!m_buffer)
	{
		InitBuffer();
	}
	AttachBufferToSlot(EyeAdaptationDataInputOutputSlotName, m_buffer);
}
```
# AttachmentImage 资产

AttachmentImage 也可以通过资产（.attimage 文件）创建，具有一个资产到一个实例的关系。并且可以在通道模板的图像附件中引用该资产。

Atom 示例中的一个用例是使用 brdf 查找映像。与临时图像的声明类似，如果通道模板中的通道附件使用以下数据，则此持久附件图像由 pass 管理。
```JSON
	  "ImageAttachments": [
                {
                    "Name": "BRDFTexture",
                    "Lifetime" :  "Imported",
                    "AssetRef": {
                        "FilePath": "Textures/BRDFTexture.attimage"
                    }
                }
            ],
```
