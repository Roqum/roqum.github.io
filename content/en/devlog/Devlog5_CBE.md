+++
date = '2024-06-10T18:47:54+02:00'
title = '#5 Inventory & Item System (Chained by Eternity)'
author = 'David'
draft = false
+++

> **Hey There!**  
> Thanks for taking the time to read my devlog.  
> I would really appreciate any kind of **feedback** - whether it’s about the project, my programming, or even my writing style - **it really helps!**
> And if you just feel like chatting about game development or want to connect, feel free to reach out.
> You can find my contact info [here](https://david-burgstaller.de/about/)
>
> This devlog covers the development of my project, [Chained by Eternity](https://www.david-burgstaller.de/project/chainedbyeternity/).

<br>
Items are one of the most important core pillars of any ARPG. They are deeply interwoven with nearly every major system — from combat and progression to economy and crafting. Because of that, I needed to implement the item system early on, so I could design the rest of the game systems with item integration in mind.

## Inventory

I implemented a grid-based, jigsaw-style inventory system, inspired by classics like Diablo II. The core of this system is a custom `UObject` class called `UInventoryManager`, stored in the player’s `PlayerState`.

The `InventoryManager` is responsible for all inventory logic, including:
 - Adding and removing items
 - Finding free space in the grid
 - Equipping and unequipping gear
 - Managing equipped items
 - Validating item requirements

The items are stored inside a two dimensional array representing the grid while the equippments are stored inside a map:

```cpp
TArray<TArray<UItem*>> InventoryGrid;
TMap<EEquipmentSlots, TObjectPtr<UEquipmentItem>> EquippedItems;
``` 

All of this logic is cleanly seperated from the UI, as I'm following a strict separation of concerns between gameplay logic and presentation. Communication between systems is handled via a custom event manager. The `InventoryManager` emits and responds to relevant UI events, keeping gameplay code decoupled.

### Item Structure
Items are represented as `UObjects` and store their data in a `FItemData` `FStruct`. Using `UObjects` rather than plain structs gives the ability to work with pointers, which simplifies management - for example, an empty inventory slot simply points to `nullptr`.

The item classes follow a hierarchical inheritance pattern like this:
- `UItem` is the base class for all items and is the type the inventory system operates on.
- `UEquipment` inherits from `UItem`, adding functionality for equipping and unequipping.
- `UWeapon` inherits from `UEquipment`, and adds weapon-specific behavior like granting abilities and changing animation layers when equipped.

The same inheritance pattern applies to the data structs:
- `FItemData` contains data common to all items, such as icon, size in the inventory grid, and general properties.
- `FEquipmentData` inherits from `FItemData`, adding data related to attribute modifiers and requirements for equippable items
- `UWeapon` inherits from `FEquipmentData`, extending it with weapon-specific data like the mesh, granted abilities, and animation montages.

Each data struct is made compatible with a DataTable by inheriting from FTableRowBase, allowing me to create separate databases for each item type to keep things organized.

I’m not yet sure if this is the most elegant approach, but after experimenting with several different setups, this one has proven to be both functional and relatively clean so far.

Linking the `UWeapon` item to the actual weapon `AActor` I created during the last devlog worked without any issues. The same goes for implementing a `ItemPickUpActor` class, which holds a `UItem` instance, visually represents it in the world, and allows it to be picked up by the player.

So for now, I’m happy with how the system is coming together. If it turns out to be problematic later on, I'll learn from the mistakes I made for the future.

Down below, you can see the inventory system in action. The item in the inventory is directly linked to the weapon actor the player is holding. Equipping a different weapon not only swaps the mesh but also dynamically changes the animation layer the player uses.

I’ve implemented an animation layer system similar to what’s shown in Unreal’s Lyra sample project. Since that system is already well-documented by many others, I won’t cover it in a separate devlog.

<img src="/images/ChainedByEternity/ChangeWeapon.gif" alt="Inventory" style="display: block; width: 75%; margin: 0 auto;">

### Equipment Modifiers
If you noticed in the (admittedly low-quality) GIF above, equipping the weapon also updates the attributes shown in the left panel of the inventory.   
That’s because equipment applies a `GameplayEffect` to the player when equipped. This `GameplayEffect` is created and applied at runtime.

The actual modifier properties come from a separate `DataTable`. When a new equipment item drops, one or more modifiers are randomly selected from this table and assigned to the item’s `FEquipmentData`, defining its stats.

```cpp
USTRUCT(BlueprintType)
struct FEquipmentModifiers : public FTableRowBase
{
	GENERATED_BODY()

	UPROPERTY(EditAnywhere, Category = "Modifier Settings", meta = (Tooltip = "Select infinite for basic equipments"))
	EGameplayEffectDurationType ModifierDuration;

	UPROPERTY(EditAnywhere, Category = "Modifier Settings")
	FGameplayAttribute EffectedAttribute;

	UPROPERTY(EditAnywhere, Category = "Modifier Settings", meta = (Tooltip = "How should the attribute be effected by the value."))
	TEnumAsByte<EGameplayModOp::Type> ModifierOperation;

	UPROPERTY(EditAnywhere, Category = "Modifier Settings")
	FGameplayEffectModifierMagnitude ModifierValue;

	UPROPERTY(EditAnywhere, Category = "UI Representation")
	EValueRepresentationType ValueRepresentationType;

	FEquipmentModifiers()
		: ModifierDuration(EGameplayEffectDurationType::Infinite), ModifierOperation(EGameplayModOp::Additive), ModifierValue(FScalableFloat(0.f))
	{
	}
};
```

Doing it this way lays the foundation for a more advanced and dynamic item drop system in the future—one where rarer items can have more modifiers, or where certain modifiers are weighted to appear less frequently, making them more powerful and valuable.

I wanted to kept the modifiers separate from the item itself to maintain a modular and flexible system.

### Future Plans

Later on, I plan to implement a `Consumable` item type that stores a reference to a `GameplayAbility`, which can be activated when the item is used. This approach would allow me to support items like health or mana potions, but also open the door for more creative mechanics—like throwable bombs, temporary buff potions, or teleport scrolls.  
That said, I’ll wait and see what actually makes sense from a game design perspective before going too wild with ideas.

