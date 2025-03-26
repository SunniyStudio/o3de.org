---
title: "[DCCsi]-DccScriptingInterface---Own-Your-DCC-Toolchain"
description: ""
toc: false
---

This page outlines basic information about a DCC-Agnostic Python Framework (as a Gem) known as the DccScriptingInterface (aka DCCsi)

The DCCsi is located in a location similar to the following:
* C:\Depot\o3de-engine\Gems\AtomLyIntegration\TechnicalArt\DccScriptingInterface

The DCCsi is related to multiple light-weight integrations for DCC applications and their Python APIs/SDKs.

# Introduction

The DCCsi is a 'Digital Content Creation' (DCC) independent solution for working with O3DE Atom assets such as meshes, materials and textures. In Atom, we are providing a Python driven arbitrary data layer for technical artists to create multi-point pipelines and workflow tools that provide inter-op between O3DE editors and multiple DCC applications, as well as a framework for standalone or embedded utils and tools (Qt\PySide2.) We intend to provide sample DCC module templates, for instance theoretically a 'Substance Builder' (API and tools for materials, which would use the Substance Automation Toolkit 'SAT' python API to gen Atom materials.)

Providing this framework promotes and assists with the creation of custom tools and pipeline processes for content creators. The framework intends to support many DCC tools and their source files like Substance .sbsar (materials) and Maya for mesh/scene intermediate formats such as FBX and glTF.

## Brief
Why the name 'DCC Scripting Interface'?  Atom calls a lot of their APIs an 'Interface', and we are collaborating with other initiatives in O3dE such as the 'Python Editor Bindings' (editor scripting initiative) to flesh out some initial use-cases and prototyping using DDC tools like Maya (for modeling), and Substance (materials), and Python to prototype a full-fledged material authoring workflow. So 'DCC Tools' + 'Python Scripting' + 'Atom Interface'.

### Goals
We intend to develop the DCCsi with goals similar to the following:

* An out-of-box, shared development environment for technical artists, oriented to working with Python across a number of DCC tools, and leverage the existing python ecosystem for technical art.
* Use DCC apps like Blender, Maya or Substance in the Python driven VFX and Games ecosystem, to 'extend O3DE', and unlock its potential for content creators, and the Technical Artists that service them.
* Show how to deepen 'DCC Tool Integrations' by building custom solutions and workflows utilizing extensibility like the 'Substance Automation Toolkit' (A python based SDK and APIs for working with substance files.)
* Full material and workflow validation for 'PBR Rendering', and 'End-to-End Substance Workflow', and WYSIWYG 'Synced Look Development' across the tool chain.

### Tenets
* Interoperability - Design modules to work together efficiently and intuitively.
* Encapsulation - Define a module in terms of its essential features and interface to other components, to facilitate logical layered design and easy maintenance.
* Extensibility - Design the tool set to be easily extensible with new functionality and new tools. Individual pieces should have a generic communication mechanism to allow newly written tools to slot cleanly and transparently into the tool chain.

***
# Current Scope
The following guide the direction of developing the DCCsi Cross-DCC Python Scripting Interface (there is more research to be done)
## Pain Points
* Legacy Lumberyard DCC plugins and env for tools like Maya were installed and set globally, it was a lot of developer friction to switch environments or even branches, or change configurations per-project.
* Using Maya.env is an archaic method of environment management and bootstrapping.  This approach was really intended for only the user to modify the Maya environment on their local machine (for professional production environments there are much better ways to go about it.)
* Hard-coded environment/installation and usage of the maya.env makes branch and project hopping burdensome and error prone, doesn't provide a shared/distributable project configuration for DCC tools.
* Lumberyard had little focus on 'technical art' development and advancement, was not python driven, provided no easy-access to an IDE environment for writing tools and debugging during development
* Lumberyard had no common or shared python based frameworks to lower the barrier of entry for custom workflow or pipeline scripting and automation
* Legacy Lumberyard DCC content work required plugins (for .cgf export), what tools provided were pre-compiled and C++ based, they had no definitive owner, they were difficult to maintain, they were not kept up to date. Later versions of Lumberyard no longer needed them (moved to FBX pipeline), but the ability to develop studio tools and extensions for the O3DE pipelines and Editor is still of value for many customers.
* Before O3DE, Lumberyard Technical Artists could not maintain, customize, or extend their own domain easily (they were bottlenecked by C++ and Engineering support), the DCCsi empower the TechArt and python community to join the development efforts and customize or extend O3DE.
* Many Technical Artists we have interviewed over the years have wanted the same, we have evidence of many game teams building their own studio tools utilizing the python ecosystem, and we would like to make it easier to do this work by providing a starting point and useful frameworks.

## In Scope (Short Term)
* We consider the current state of the DCCsi to be a bare bones scaffolding and 'functional prototype'
* The DCCsi will remain non-destructive to host system, globals, and existing tools/environment configuration.
* The ability to disable and bypass all Amazon and/or O3DE customizations, returning to vanilla DCC.
* Provide a multi-point core framework that is intended to be dcc-tool agnostic (shared pure python api and supports multiple dcc apps, thus multi-point)
* Provide both generic branch and project agnostic configuration for services and tools (data driven configuration)
* Make it easy for developers to hop between branches, or projects - for example Maya can be running and bootstrapped more than once, with different configurations that are project based.
* Per-project easy provisioning (run a tool from the framework to provision the default bootstrapping config and launcher, etc)
* Project shared 'DCC Configuration' (submitted in project data source control) to be distributed, ensure everyone is running the same per-project and per-dcc config, in a local project env context environment (which provides the project data-driven bootstrapping)
* Is capable of bootstrapping multiple DCCs, in the context of a O3DE Project (environment) Now: Maya, Blender and Substance (later others: like Houdini, 3dsMax, etc.)
* Build to a fully featured development environment and scripting api, for tool, workflow and pipeline development. Support IDEs along with project files and solutions, as well as development features like sandboxing.
* Wing IDE integrated, and working to provide additional IDE support (examples) to develop in PyCharm and VScode, with auto-connect and remote debugging.
* Python 3.x compliant to combat approaching 2.7 end of life deadline
* Currently Windows only (but we should be writing code in a cross platform way.)
* Hosted on GitHub (always be able to pull the latest distribution) and promote community driven extensions (we hope this lights the pilot light of extensibility)

## Out of Scope (Help us build the FUTURE)
* OS Agnostic. Run everywhere Maya or other DCC tools can install (the existing prototype windows only, we need community support to make OS agnostic)
* Stability with addition of unit testing and test coverage - the next phase of development should move to to be test driven.
* Code health monitor (online dashboard, upload crash reports and stack dumps, cloudhandler for logging, etc.)
* API and feature versioning and proper branching workflows, allowing feature divergence from production code.
* More out-of-box developer env setups and IDE integrations: Eclipse(pydev) or others.
* Support for more DCC tools: Houdini, 3dsMax, etc. (these will come both soon and as needed based on the communities priorities and help)

***

(More to come)

![DCCsi_Diagram](https://github.com/HogJonny-AMZN/O3DE/blob/main/Images/DCCsi/dcc_scripting_interface_diagram.png)

