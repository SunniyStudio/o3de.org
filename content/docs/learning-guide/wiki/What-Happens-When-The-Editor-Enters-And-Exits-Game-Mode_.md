---
title: "What-Happens-When-The-Editor-Enters-And-Exits-Game-Mode_"
description: ""
toc: false
---

Answer from [nick-l-o3de](https://github.com/nick-l-o3de):

IIRC, When the editor enters game mode, it deactivates all entities in the world and then for each such entity, it creates a new game entity, and then for each component on the original entity, calls `BuildGameEntity()` on the (not active) editor component giving it a chance to make a game component and copy any information and configuration to it.

This gives you the opportunity to have very complicated and heavy editor components and very lightweight game components for runtime, carrying only the data necessary to function, as well as an opportunity to optimize the data in those components for runtime, quantize it, or whatever.

Once all the entities have been created and components converted via `BuildGameEntity()`, then those game entities thus created are activated and the game runs. 
when the game mode ends, those temp entities are destroyed and the original editor entities are re-activated
there are additional complexities involving prefabs, but ultimately ,the prefabs are instantiated into a big list of editor entities with editor components, and then those editor components are deactivated and turned into game components via `BuildGameEntity()`, and those game components are what gets activated
the serializer is involved I think, which means that if you are losing data it might be a number of things, like, the serializer not knowing about those objects or fields, or, it might be a problem where the `BuildGameEntity()` function is not copying the config over.
