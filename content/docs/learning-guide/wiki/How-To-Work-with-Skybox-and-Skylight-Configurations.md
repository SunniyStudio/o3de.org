---
title: "[How-To]-Work-with-Skybox-and-Skylight-Configurations"
description: ""
toc: false
---

Understanding: there are two independent but related component operators here
- HDRi Skybox Component, this component **only** renders the backdrop skybox pass (and does not contribute light.)
- Global Skylight (IBL), this component only injects 'image based lighting' into the scene:
- - indirect diffuse lighting (global also, feeds into the lighting system)
- - specular reflections (global fallback, blends with local reflection probes)

These can be used independently, they can be used together:
- combined is likely the most common configuration (both entities on one component)
- another user choice would be 2 entities, 1) a skybox entity (skybox component only), 2) a Skylight entity (skylight component only), these could be separated in the hierarchy, or one nested under the other (skybox child of skylight)

These configurations give you all of the options you need to hide/show-enable/disable 
- any entity can be hidden in the outliner (eyeball)
- If both components are on the same entity, both will be hidden (their effect disabled)
- if they are on two separate entities that are not nested in the hierarchy, each can be hidden independently of the other
- if they are on two separate entities nested (parent child):
- - the child can be hidden independently of the parent
- - if the parent is hidden it will hide the child as well

Additionally, there is a mechanism to independently control the active state of any _component_, and this component state is independent of the outliner mechanism to _hide/show an entity_
If both components are on the same entity:
- select and right-click on either component and select 'disable component' (or enable)

![image](https://user-images.githubusercontent.com/23222931/151415658-7bd7c126-3f73-4663-a7e1-e9466857668d.png)

You are not restricted from having more then one of the same component on an entity (you can have two skybox components), the restriction is that only one of them can be active at a time.

![image](https://user-images.githubusercontent.com/23222931/151417632-542c3deb-f69a-43e3-8702-8c11e7d7f440.png)

You are in control

![image](https://user-images.githubusercontent.com/23222931/151417739-e90a54eb-5a36-45e5-8fa6-a3e9302969b9.png)

There are endless options and many configurations.  You can for instance, use the 'indirect diffuse cubemap' as the skybox, this will give you a relatively flat and soft skybox backdrop (but can retain some of the lighting qualities and color ... this is exactly how the alternate lighting presets work in the material editor)

![image](https://user-images.githubusercontent.com/23222931/151418477-b0cd5f49-eff1-4a4c-b561-9531f5470d43.png)

There is not currently an easy way to alter the 'clear color' (no draw) but there are viable and not difficult workarounds.  It would be trivial for you or anyone else, to simply create a library of low-resolution .exr images (add the filemask _iblskyboxcm.exr) of various colors, These can easily be added to one or more skybox components, then toggle between them.
