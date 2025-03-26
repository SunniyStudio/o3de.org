---
title: "O3DE-Standard-repo-formats"
description: ""
toc: false
---

## O3DE repos have two standard formats:
1. Singular object repos
2. Compound object repos

## Singular object repos:
All singular object repos represent 1 top level object. This does not mean that the repo only consists of one object, but one parent object that all other objects are children of. A good example of this is the o3de/o3de core engine repo. The engine itself is an object, an engine object. The engine has many children objects in that repo but only 1 top level object. These repos are easy to spot because they have a O3DE <object type>.json in the root. They may have other json files in the root, a common is the .automatedtesting.json. For the example above, the engine has an engine.json in the root:
```
<root>\
    <object type>.json
    <.automatedtesting.json>
    ...
```
Example:
```
o3de\
    engine.json
```

## Compound object repos:
All compound repos represent many top level objects. This does not mean that each top level object has no child objects, just that there is no single parent of all of these objects in the repo. A good example of this is the o3de-extras repo. These repos are easy to spot because they have no <object type>.json in the root. They may have other json in the root, like the .automatedtesting.json, but not an <o3de type>.json. The standard format for a o3de compound repo has folders under the root that have the name of the contained top level objects:
```
<root>\
    <Engines>\
        <An Engine>\
            engine.json
        <Another Engine>\
            engine.json
        ...
    <Projects>\
        <A Project>\
            project.json
        <Another Project>\
            project.json
        ...
    <Gems>\
        <A Gem>\
            gem.json
        <Another Gem>\
            gem.json
    ...
    <Templates>\
        <A Template>\
            template.json
        <Another Template>\
            template.json
        ...
    <Restricted>\
        <A Restricted>\
            restricted.json
        <Another Restricted>\
            restricted.json
        ...
    <.automatedtesting.json>
```
Example:
```
o3de-extras\
    Gems\
        XR\
            gem.json
        OpenXrVk\
            gem.json
    Projects\
        OpenXrTest\
            project.json
    .automatedtesting.json
```           
## .automatedtesting.json
The automatedtesting.json is an optional file for the O3DE Automated Review (AR). Jenkins reads these files and executes them before an AR run. Note that each section is optional, so any empty list need no be in there, but doesn't hurt to have anyway. They have a format:
```
{
    "REGISTER_ENGINES": [
        <a relative path from the root to an engine in this repo>
        <another relative path from the root to an engine in this repo>
        ...
    ],
    "REGISTER_PROJECTS": [
        <a relative path from the root to a project in this repo>
        <another relative path from the root to a project in this repo>
        ...
    ],
    "REGISTER_GEMS": [
        <a relative path from the root to a gem in this repo>
        <another relative path from the root to a gem in this repo>
        ...
    ],
    "REGISTER_TEMPLATES": [
        <a relative path from the root to a template in this repo>
        <another relative path from the root to a template in this repo>
        ...
    ],
    "REGISTER_RESTRICTED": [
        <a relative path from the root to a restricted in this repo>
        <another relative path from the root to a restricted in this repo>
        ...
    ],
    "ENABLE_GEMS": [
        <Name of a gem in this repo to enable for the AutomatedTesting project so it tests are run>
        <another Name of a gem in this repo to enable for the AutomatedTesting project so it tests are run>
        ...
    ],
    "REPOS": [
        {
            "NAME": <Name of a dependent repo>,
            "URL": <URL of a dependent repo>,
            "BRANCH": <Branch of a dependent repo>
        },
        {
            "NAME": <Name of another dependent repo>,
            "URL": <URL of another dependent repo>,
            "BRANCH": <Branch of another dependent repo>
        },
        ...
    ]
}
```
Example of the engines .automatedtesting.json:
```
{
    "REGISTER_ENGINES": [
    ],
    "REGISTER_PROJECTS": [
    ],
    "REGISTER_GEMS": [
    ],
    "REGISTER_TEMPLATES": [
    ],
    "REGISTER_RESTRICTED": [
    ],
    "ENABLE_GEMS": [
    ],
    "REPOS": [
        {
            "NAME": "o3de-extras",
            "URL": "https://github.com/o3de/o3de-extras.git",
            "BRANCH": "development"
        }
    ]
}
```
This says the engine has a dependent repo, o3de-extras. Therefore whenever we run AR the engine will pull in o3de-extras development branch.

Example of o3de-extras .automatestesting.json
```
{
    "REGISTER_ENGINES": [
    ],
    "REGISTER_PROJECTS": [
        "Projects/OpenXRTest"
    ],
    "REGISTER_GEMS": [
        "Gems/XR",
        "Gems/OpenXRVk"
    ],
    "REGISTER_TEMPLATES": [
        "Templates/Multiplayer"
    ],
    "REGISTER_RESTRICTED": [
    ],
    "ENABLE_GEMS": [
        "Gems/XR",
        "Gems/OpenXRVk"
    ]
}
```
When o3de-extras is pulled in it will execute this .automatedtesting.json which says it has a number of top level objects to register and wants to enable two gems for testing. As we see here this file omits the "REPOS" section which is fine because all sections are optional.

Not only are the the section optional, the entire .automatesting.json is an optional file. In testing or the dependent repos, if the repo has no .automatesting.json but otherwise follow the standard O3DE repo format, AR will assume this means you want all objects registered and enabled for testing.
