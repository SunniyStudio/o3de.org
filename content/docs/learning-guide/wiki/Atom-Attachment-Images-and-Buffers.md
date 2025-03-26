---
title: "[Atom]-Attachment-Images-and-Buffers"
description: ""
toc: false
---

# Overview
If a gpu resource (image, buffer) can be writable at any point of its lifetime, it needs to be declared (used) as pass attachment for a pass if there are any draw items submitted in this pass may use this resource. These kind of resources are called attachment images or attachment buffers.</p><p>Some resources' lifetime are only needed in the period of one frame, which we call them transient resources. And the other resources which have lifetime longer than one frame we would call them persistent resources. For transient resources, they are already implemented in Pass class which a pass can create them (owned attachment) during the frame and have them be passed around to other connection passes. For persistent resources, there are different ways to create them and attach them to passes.
In this document, we will explain how to work with both transient and persistent attachment resources.
# Using An Attachment
In this section, it explains how an attachment is used in a frame. The details are already implemented via Pass's PassAttachment. But it can be helpful when adding a customized pass or for debugging.
To use a gpu resource as an attachment, the resource need to be imported to frame's attachment database via `RHI::FrameGraphAttachmentInterface`'s `ImportBuffer()` or `ImportImage()` function before the frame's render starts. And for each Pass (Scope) which uses this attachment, it needs to call `RHI::FrameGraphInterface's UseAttachment()` function with the resource. And for the last thing, any shader resource groups uses this resource need to bind its resource view to the shader resource groups.
The import attachment step is to tell the frame graph that this resource will be used as attachment in this frame. The use attachment step is to tell the frame graph, how this attachment will be used for this scope. For example, used for read or write.
# Transient Attachments
Assume in the rendering pipeline, it needs to render a depth image first and this depth image will used as input for other passes such as light culling pass and forward pass in the same frame. Although, this depth image doesn't need to be preserved after this frame which make it a transient attachment.
For such an image, we only need to declare it in pass template's ImageAttachments data. The following data block is part of the .pass file for a pass template. The pass created with this pass template will create a transient image and import it into the frame as well as declare using it for the scope.

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
Note: to find more about how pass handles transient resources, you can search Pass::CreateTransientAttachments in Pass.cpp file.

# Persistent Attachments

Unlike transient attachments, which are created and managed by pass, the resources used as persistent attachments are usually created manually, then attached to pass slots which one exception that an AttachmentImage Asset is used. 

In RPI, the AttachmentImage class (derived class of RPI::Image) is used to represent image used as attachment and the Buffer class is used to represent attachment buffer.  After these resource objects are created, they can be attach to pass by using Pass::AttachBufferToSlot or Pass::AttachImageToSlot functions. 

The creation of the resource objects are usually happened in feature's implementation, for example in feature processors' initialization. Attaching these resources to passes has to happen inside pass's BuildAttachmentsInternal virtual function. 

For example, a buffer which saves luminance values of history frames is needed for eye adaption feature with both write and read operations. Since the values are calculated and saved on GPU and required to be saved across multiple frames, this buffer needs to be a persistent attachment buffer. 

The following block shows how the buffer is created and used for pass and related shader. 
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
# AttachmentImage Asset

AttachmentImage can be created via asset (.attimage file) as well, with one asset to one instance relationship. And the asset can be referenced in pass template's image attachment. 

One use case in Atom's samples is the usage of brdf lookup image. Similar to transient image's declaration, this persistent attachment image is managed by pass if the pass attachment in pass template is using the following data. 
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
