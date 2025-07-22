+++
date = '2024-06-10T18:47:54+02:00'
title = '#3 Inventory & Item System (Chained by Eternity)'
author = 'David'
draft = true
+++

Project of the devlog: [Chained by Eternity](https://www.david-burgstaller.de/project/chainedbyeternity/)

One of the first things I implemented after the attribute set of my character was the inventory system since items play a major role in an ARPG. Almost everything is based or influenced by items, so I decided to take them on early.

## Inventory and Item System

We use a grid-based, jigsaw-style inventory system in our game, inspired by classics like Diablo II. The core of this system is a custom `UObject` class called `InventoryManager`, stored in the player’s `PlayerState`.

The InventoryManager is responsible for all inventory logic, including:
 - Adding and removing items
 - Finding free space in the grid
 - Equipping and unequipping gear
 - Validating item requirements

All of this logic is cleanly seperated from the UI since we are following a strict separation of concerns between gameplay logic and presentation. Communication between the systems is handled via an custom event system. The `InventoryManager` emits and responds to relevant UI events, keeping gameplay code decoupled.

### Item Structure
Items are represented as `UObjects` and store their data in a `FItemData` `FStruct`. Using `UObjects` rather than plain structs gives us the ability to work with pointers, which simplifies management - for example, an empty inventory slot simply points to `nullptr`.

Our item classes follow a hierarchical inheritance pattern like this:
- `UItem` is the base class for all items and is the type the inventory system operates on.
- `UEquipment` inherits from `UItem`, adding functionality for equipping and unequipping.
- `UWeapon` inherits from `UEquipment`, and adds weapon-specific behavior like granting abilities and changing animation layers when equipped.

<img src="/images/ChainedByEternity/ChangeWeapon.gif" alt="Inventory" style="display: block; width: 65%; margin: 0 auto;">

### Gameplay Integration
We use the Gameplay Ability System (GAS) to implement our item effects. For Example:
  - Character Attribute Manipulation
    Each equipment stores an array of `GameplayEffectModifiers`. When equipped, a `GameplayEffect` is created dynamically, filled with those modifiers, and applied to the character.
  - Granting Abilities
    Our Weapon stores an array `GameplayAbility` classes that is granted to the character when equipped.
  - Consumables
    Items like health or mana potions don’t grant abilities - instead, they activate an ability instantly, such as applying a healing effect on use.

### Design Benefits

By seperating items from their effects, we've built a flexible and scalable system. Creating new item types becomes straightforward. Even more complex item behaivios like this are still easy to handle: 

- Granting passive or active abilities
- Triggering complex effects (e.g., throwing bombs, summoning allies)
- Providing conditional effects (e.g., immunity to specific damage types)

