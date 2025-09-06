# Don't Starve - Decompiled Source Code

This repository contains a decompiled version of the Lua source code for the popular survival game, **Don't Starve**. The primary purpose of this project is to serve as a resource for education, research, and modding. By examining this code, developers and enthusiasts can gain a deeper understanding of the game's mechanics, AI, and overall architecture.

## ❗ Disclaimer

This project is not affiliated with or endorsed by Klei Entertainment, the creators of Don't Starve. The code in this repository was obtained through decompilation and is intended for educational and research purposes only. It is not intended for the creation of unauthorized copies of the game or for any other commercial use. Please support the developers by purchasing the game if you have not already.

## 📁 Directory Structure

The repository is organized into several key directories. Here's a breakdown of the most important ones:

-   `data/`: This is the main directory for game assets. It contains everything from animations (`anim`) and images (`images`) to sounds (`sound`) and the core game scripts (`scripts`).
-   `data/scripts/`: The heart of the game's logic. This directory contains all the Lua scripts that define the game's mechanics, entities, and systems.
-   `data/scripts/prefabs/`: Contains the definitions for all the "prefabs" in the game. A prefab is a template for a game object, such as a character, a monster, an item, or a piece of scenery.
-   `data/scripts/components/`: This directory holds the various components that can be attached to entities to give them specific behaviors and attributes (e.g., `health`, `inventory`, `combat`).
-   `data/scripts/brains/`: This is where the AI for the game's creatures is defined. Each brain uses a behavior tree to control the actions of a creature.
-   `mods/`: This directory contains examples of how to create mods for the game. It includes sample mods for adding characters, prefabs, and custom components.
-   `docs/`: Contains additional documentation, such as an explanation of the behavior tree system used in the game.

## ⚙️ Core Engine Concepts

The game's engine is built on a few key concepts that are important to understand before diving into the code.

### Entity-Component System (ECS)

Don't Starve uses an Entity-Component System (ECS) architecture. This is a design pattern that favors composition over inheritance.

-   **Entities:** An entity is a general-purpose object. In Don't Starve, anything from the player character to a piece of flint on the ground is an entity.
-   **Components:** Components are chunks of data that can be attached to entities. For example, a `health` component could be attached to any entity that can take damage.
-   **Systems:** Systems are the logic that operates on entities with specific components. For example, a `combatsystem` would handle interactions between entities that have `health` and `combat` components.

You can find the component definitions in `data/scripts/components/`.

### Behavior Trees for AI

The artificial intelligence for all the creatures in the game is implemented using behavior trees. A behavior tree is a hierarchical model of decision-making.

-   The core implementation of the behavior tree can be found in `data/scripts/behaviourtree.lua`.
-   The AI logic for each creature is defined in its "brain," which can be found in `data/scripts/brains/`.
-   The `docs/` directory contains a detailed breakdown of the behavior tree code.

### Event-Driven Logic

Much of the game's logic is event-driven. This means that different parts of the code can "listen" for specific events and then react to them.

-   Events are used for everything from a character taking damage to a change in the time of day.
-   The `ListenForEvent` function is used to register a callback for a specific event.
-   This creates a decoupled architecture where different systems can interact without having direct dependencies on each other.

## 🔧 Modding Guide

Don't Starve has a powerful and flexible modding system. The `mods/` directory contains several examples that you can use as a starting point. Here's a guide to creating your own mod, based on the `samplemod`.

### `modinfo.lua`

This file contains the metadata for your mod. It's a simple Lua script that defines a few key variables:

-   `name`: The name of your mod.
-   `description`: A brief description of what your mod does.
-   `author`: Your name.
-   `version`: The version number of your mod.
-   `forumthread`: A link to the mod's thread on the Klei forums.
-   `api_version`: The API version that your mod is compatible with.

### `modmain.lua`

This is the main entry point for your mod's logic. It's where you'll hook into the game to make your changes. Here are some of the key functions you can use:

-   `AddPrefabPostInit(prefab_name, fn)`: This function allows you to run a custom function after a specific prefab has been initialized. This is useful for modifying existing game objects.
-   `AddComponentPostInit(component_name, fn)`: Similar to the above, but for components. This lets you modify the behavior of a component.
-   `AddSimPostInit(fn)`: This function runs once after the simulation has been initialized.
-   `AddGamePostInit(fn)`: This function runs once after the game has been initialized.

### Example: Modifying Wilson

Here's an example from `samplemod/modmain.lua` that shows how to modify the character Wilson:

```lua
function wilsonpostinit(inst)
    -- Halve Wilson's max hunger
    inst.components.hunger:SetMax(TUNING.WILSON_HUNGER * 0.5)

    -- Add a custom component
    inst:AddComponent("myowncomponent")
end

AddPrefabPostInit("wilson", wilsonpostinit)
```

In this example, the `wilsonpostinit` function is registered to run after the "wilson" prefab has been initialized. It then changes Wilson's max hunger and adds a new custom component.

### Modifying Tuning Variables

You can directly override the game's tuning variables in your `modmain.lua` file. For example:

```lua
-- Halve Wilson's default hunger
TUNING.WILSON_HUNGER = 50
```

This will change the value of `WILSON_HUNGER` for the entire game.

## 🚀 How to Use This Repository

This repository is a valuable resource for anyone who wants to learn more about how Don't Starve works. Here are a few suggestions for how to get started:

-   **Explore the `prefabs`:** The `data/scripts/prefabs/` directory is a great place to start. Pick a creature or an item that you're familiar with and read through its source code. This will give you a good idea of how game objects are constructed.
-   **Read the `brains`:** If you're interested in AI, check out the `data/scripts/brains/` directory. You can see how the behavior trees are used to create the complex and often unpredictable behavior of the game's creatures.
-   **Try making a mod:** The best way to learn is by doing. Try creating a simple mod that changes a tuning variable or modifies a character. Use the `samplemod` as a guide.
-   **Trace the code:** Pick an action in the game, like chopping down a tree, and try to trace the code that makes it happen. Start with the `actions.lua` file and follow the chain of events.

## 🙌 Contributing

This project is a community effort, and contributions are welcome! Here are a few ways you can help:

-   **Improve the documentation:** If you figure out how a particular system works, please consider adding to this README or writing a new document in the `docs/` directory.
-   **Add comments to the code:** Much of the code is uncommented. Adding comments to explain how the code works would be a huge help to others.
-   **Fix bugs in the decompiled source:** The decompilation process is not perfect, and there may be bugs or errors in the code. If you find and fix one, please submit a pull request.
