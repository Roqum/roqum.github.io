+++
title = 'Chained By Eternity (Main Project)'
date = '2025-06-26T12:39:17+02:00'
draft = false
tocVisible = true
categories = ['Unreal Engine 5', 'Gameplay Ability System', 'C++']
projectDescription = ["A vertical slice of a classic action RPG, built in Unreal Engine 5 using the Gameplay Ability System. Started in March 2024 as a long-term project. It showcases all core mechanices of a classic RPG like real-time combat, character progression, ability system and much more."]
+++

## Project Info

<br>
<div style="display: grid; grid-template-columns: 150px 1fr; ">
  <div>Status:</div><div>Currently in progress...</div>
  <div>Started:</div><div>17. February 2024</div>
  <div>Teamsize:</div><div>2 Programmers</div>
  <div>My Working Hours:</div><div>~400h</div>
  <div>Source Code:</div><div>Coming soon</div>
  <div>Demo:</div><div>Coming soon</div>

</div>
<br>

> **Note:** \
This project is built using **Unreal Engine 5 (UE5)** and the **Gameplay Ability System (GAS)**. We're developing almost **entirely in C++**, with minimal use of Blueprints.
Since this is a portfolio presentation and not a tutorial, I won’t go into detailed explanations of UE5 or GAS concepts.

> **Note 2:**  
> Thanks for taking the time to read about my project.  
> I would really appreciate any kind of **feedback** - whether it’s about the project, my writing style, or even my grammar.  
> Feel free to send me an email - **it really helps!**

## Intorduction

**Chained By Eternity** is my most ambitious and dedicated project to date. My goal with this project is to showcase my skills and finally land a job in the game development industry.

Together with a friend, we set out to build something technically challenging. We knew early on that building a full-scale ARPG like *Path of Exile* or *Diablo* would be unrealistic for a two-person team. Instead, we focused on creating a **high-quality vertical slice** a playable snapshot of what a complete game could be. This includes real-time combat, a fully functional ability and progression system, an inventory and item systems loot mechanics, and many other core features typically found in a classic action RPG.


## Current State

### Overview 
We've already achieved a lot, but there's still a long road ahead. Many systems are implemented and working, but not yet at the level of quality we're aiming for. We're currently focused on building a solid foundation, following the principle:

> "Make it work first, make it good later."

Please keep that in mind when reading the overview below. Most of the listed systems are functional but still need polish and refinement. More details are provided in the following sections.

#### Features Already Implemented

- **Advanced Attribute System**  
  Modular and expandable attribute system with stat scaling and interactions.
- **Multiple Weapon Types**  
  Currently supports 3 weapon types with layered animations that change dynamically when switching weapons.
- **Inventory and Equipment System**  
  Allows players to manage items, equip, and unequip equipment.
- **Items**  
  Items can modify player attributes, grant new abilities when equipped or activate abilities when used.
- **Checkpoint System**  
  Basic save/load functionality to restore the player's game state.
- **Basic Enemy AI**  
  Simple behavior logic like chasing and attacking the player.
- **Combat Abilities**  
  Including single-target, area-of-effect (AOE), and projectile-based abilities.
- **UI**  
  Functional UI for the main menu, character creation, in-game HUD, and inventory management.
- **Loot Mechanics**  
  For complex item drop calculation based on enemy level and rarity.
---

#### Big Features Still in Development / Not Yet Implemented

- **Level Environment & Art**  
  Creating visually appealing level environment.
- **Enemy Spawn System**  
  Procedurally generated spawn algorithm for random enemy placement.
- **Quest System**  
  Including dialogues, progression tracking, and rewards.
- **Advanced AI**  
  Smarter enemy behavior for diffrent enemy type (Probably going for state trees).
- **Ability & Attribute Menus (UI)**  
  User interface for managing abilities and stats.
- **General Polish**  
  Refine animations, add effects and sound, optimize performance, etc.


### Inventory and Item System

We use a grid-based, jigsaw-style inventory system in our game, inspired by classics like Diablo II. The core of this system is a custom `UObject` class called `InventoryManager`, stored in the player’s `PlayerState`.

The InventoryManager is responsible for all inventory logic, including:
 - Adding and removing items
 - Finding free space in the grid
 - Equipping and unequipping gear
 - Validating item requirements

All of this logic is cleanly seperated from the UI since we are following a strict separation of concerns between gameplay logic and presentation. Communication between the systems is handled via an custom event system. The `InventoryManager` emits and responds to relevant UI events, keeping gameplay code decoupled.

#### Item Structure
Items are represented as `UObjects` and store their data in a `FItemData` `FStruct`. Using `UObjects` rather than plain structs gives us the ability to work with pointers, which simplifies management - for example, an empty inventory slot simply points to `nullptr`.

Our item classes follow a hierarchical inheritance pattern like this:
- `UItem` is the base class for all items and is the type the inventory system operates on.
- `UEquipment` inherits from `UItem`, adding functionality for equipping and unequipping.
- `UWeapon` inherits from `UEquipment`, and adds weapon-specific behavior like granting abilities and changing animation layers when equipped.

<img src="/images/ChainedByEternity/ChangeWeapon.gif" alt="Inventory" style="display: block; width: 65%; margin: 0 auto;">

#### Gameplay Integration
We use the Gameplay Ability System (GAS) to implement our item effects. For Example:
  - Character Attribute Manipulation
    Each equipment stores an array of `GameplayEffectModifiers`. When equipped, a `GameplayEffect` is created dynamically, filled with those modifiers, and applied to the character.
  - Granting Abilities
    Our Weapon stores an array `GameplayAbility` classes that is granted to the character when equipped.
  - Consumables
    Items like health or mana potions don’t grant abilities - instead, they activate an ability instantly, such as applying a healing effect on use.

#### Design Benefits

By seperating items from their effects, we've built a flexible and scalable system. Creating new item types becomes straightforward. Even more complex item behaivios like this are still easy to handle: 

- Granting passive or active abilities
- Triggering complex effects (e.g., throwing bombs, summoning allies)
- Providing conditional effects (e.g., immunity to specific damage types)


### Player Abilities

While there's still much work to be done in this area, we’ve already implemented a foundational system that’s flexible and extensible - and at least one abilitiy that is worth showcasing.

#### Abilities Architecture

At the core of our system is a custom `UBaseGameplayAbility` class, which inherits from GAS's `UGameplayAbility`. This base class stores an input `FGameplayTag` and implements ability cooldown functionality.

This setup allows us to:
- Activate the ability by it's input `FGameplayTag`
- Dynamically bind an ability to a `InputAction`
- Let players assign abilities to their preferred input keys
- Set the cooldown duration to any value


In addition, we’ve implemented a `UDamageGameplayAbility` class that handles damage-specific abilities. This class defines:
- Raw damage value
- Damage types by `FGameplayTags`
- A custom `ApplyDamage()` function that is using a custom `UGameplayEffectExecutionCalculation` class.

This `UGameplayEffectExecutionCalculation` class is handling the complex damage calculations considering both source and target attributes. Since this class is relatively complex, I’ll describe it in more detail in a dedicated section.


#### Implemented Abilities
We currently have three working prototype abilities.

- Projectile Ability
  Fires a projectile that applies a `UGameplayEffect` on hit.
- Melee Combo Attack 
  A close-range attack that supports combo chaining 
- Rain of Arrows (AOE)
  A area-of-effect ability that spawns multiple damaging projectiles raining down a targeted area.

All of these abilities use Animation Montages for playing animations. To synchronize gameplay logic with the animation, we use Gameplay Events, which broadcast an FGameplayTag at specific animation frames.

Using GAS’s `AbilityTask_WaitGameplayTag`, we listen for these events to time key moments in the ability’s execution like spawning a hitbox or projectile at the correct moment.

##### Melee Combo Attack 
We decided to store references to the basic attack animation montages inside our weapon class - specifically, as an array of attack montages. This setup allows us to automate combo chaining directly within the ability logic.

The melee ability reads this array to:
- Determine the maximum number of combo steps
- Play each montage in sequence as the player chains attacks

To smooth out gameplay, we implemented two Gameplay Events inside each animation montage:

- One event allows the next combo attack to be triggered before the current animation finishes (early input window)
- The other resets the combo if the animation ends without input (combo timeout)

This design gives us the flexibility to define unique combo chains for each weapon, simply by assigning different montages with the appropriate events.

<img src="/images/ChainedByEternity/AttackMontage.png" alt="Inventory" style="display: block; width: 80%; margin: 0 auto;">
<br>
<img src="/images/ChainedByEternity/MeleeAttackCombo.gif" alt="Inventory" style="display: block; width: 80%; margin: 0 auto;">


##### Rain of Arrows

For this ability, we created a Niagara System that spawns multiple arrows in a circular pattern above the ground. These arrows rapidly shoot downward, "raining" onto enemies in the target area.

To enhance combat feel and realism, I wanted the ability to hit enemies multiple times, as several arrows are hitting the enemy.

I considered three possible approaches:

1. **Spawn a large hitbox** covering the area and trigger it multiple times.
2. **Predefine the arrow spawn positions**, send them to the Niagara system, and do a line trace from each point down to the ground.
3. **Listen to Niagara particle collision events** and trigger a small hitbox at each impact location.

The **first solution** would’ve been the simplest to implement - but I was aiming for something more realistic.

The **third option** felt ideal at first: it's reactive and precise. However, it's not adviced to mix VFX with gameplay logic. Even though in my implementation, the VFX would only trigger an event (and the gameplay logic responds), this approach can cause problems - especially in multiplayer games. That’s because VFX systems like Niagara are often client-side only and not run on the server, meaning they can desynchronize from core gameplay.

So in theory, the **second solution** is the cleanest: define spawn locations up front, trigger the VFX based on them, and handle all gameplay logic (e.g., line traces, damage) independently and safely.

> **But… I still went with the third option.** 😅  
> Why?  
> Because we're making a single-player game, and I wanted it to get done before going on vacation! 😄

Custom ability task for area-of-effect selection using a decal:
<img src="/images/ChainedByEternity/RainOfArrowAreaSelection.gif" alt="Inventory" style="display: block; width: 80%; margin: 0 auto;">

Rain of Arrows damaging enemies showcase:
<img src="/images/ChainedByEternity/RainOfArrowDamage.gif" alt="Inventory" style="display: block; width: 80%; margin: 0 auto;">


### To be continued...
This project page isn’t finished - there’s still a lot more to present. I’ll continue adding new chapters over time.