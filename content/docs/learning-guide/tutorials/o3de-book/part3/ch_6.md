---
linkTitle: 第6章 AZ::Interface是什么
title: 第6章 AZ::Interface是什么
description: 第6章 AZ::Interface是什么
---
# 第6章 AZ::Interface是什么

## Introduction
The majority of component communication in O3DE occurs using AZ::EBus. It is the most powerful, the most generalized and the most feature rich inter-component communication system in the engine. I could even say that once you understand AZ::EBus you will understand how to work with the engine. However, I will start by showing you the simpler models first - AZ::Interface in this chapter and then AZ::Event in the next chapter. By studying these two first you will be ready to understand AZ::EBus.

{{<note>}}
You can find the code and project changes for this chapter on GitHub:
https://github.com/AMZN-Olex/O3DEBookCode2111/tree/ch06_az_interface
{{</note>}}

AZ::Interaface in O3DE is a fancy singleton that works across module boundaries. It allows you declare an interface and then implement it in one of your objects. However, since AZ::Interface is, essentially, a singleton, a component that acts as an interface handler has to be the only component that handles that interface.

{{<note>}}
You can find the documentation on AZ::Inteface online at
https://www.o3de.org/docs/user-guide/programming/az-interface/
{{</note>}}

## Level Components
That brings us to a component type that is naturally unique and would serve as a great candidate to handle an AZ::Interface: Level components. We have already seen regular game components that can be added to entities in the Editor. There is another type of entity hidden in plain sight in Entity Outliner.

Figure 6.1. Accessing Level entity and its components

![](/images/learning-guide/tutorials/o3de-book/Part3/o3de_book_3_3.PNG)














