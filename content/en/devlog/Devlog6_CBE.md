+++
date = '2024-06-10T18:47:54+02:00'
title = '#2 Player Abilities'
author = 'David'
draft = true
+++

> **Hey There!**  
> Thanks for taking the time to read my devlog.  
> I would really appreciate any kind of **feedback** - whether it’s about the project, my programming, or even my writing style - **it really helps!**
> And if you just feel like chatting about game development or want to connect, feel free to reach out.
> You can find my contact info [here](https://david-burgstaller.de/about/)
>
> This devlog covers the development of my project, [Chained by Eternity](https://www.david-burgstaller.de/project/chainedbyeternity/).

<br>
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

