+++
date = '2024-06-10T18:47:54+02:00'
title = '#3 Attribute System (Chained by Eternity)'
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

What would an RPG be without a proper attribute system?
Probably hard to call it a real RPG then.

In this devlog, I’ll implement my very first attributes using the **Gameplay Ability System (GAS)**. This initial setup will likely evolve a lot over time, but it’s important to get the basics in place so I can begin proper testing.

We’ll likely revisit and optimize the system in future devlogs.

We will probably revisit and optimize it in later devlogs.

### Attribute Set

I've decided to split my attribute in three categories and make a kind of hierarchy.

- **Primary Attributes** 
- **Secondary Attributes**
- **Vital Attributes**

This hierarchy flows from top to bottom:

- **Primary Attributes** are fundamental and self-contained.
- **Secondary Attributes** are derived from Primary Attributes.
- **Vital Attributes** (like Health, Mana, Stamina) may depend on both Primary and Secondary Attributes.

No attribute higher in the hierarchy should depend on those below it. That would cause problems with initialization order and update logic. By keeping dependencies flowing downward only, the system stays clean and predictable.

I started with the following basic attributes: 
<div style="display: flex; justify-content: space-evenly;">
<div>

**Primary Attributes**
- Stregth
- Intelligance
- Constitution
- Dexterity
</div>
<div>

**Secondary Attributes**
- Max Health
- Max Mana
- Armor
- Fire Resistance
- Ice Resistance
- Arcane Resistance
- Critical Chance
- Critical Damage
</div>
<div>

**Vital Attributes**
- Health
- Mana
</div>
</div>

##### Implementation

Unreal's Gameplay Ability System provides a built-in `UAttributeSet` class where `Attrbiutes` are definined and managed . These `Attributes` consist of two float values, a `CurrentValue` and a `BaseValue`. The `BaseValue` is the actual value of the attribute while the `CurrentValue` is the base value modified by temporary influences like `GameplayEffects`.

I started by creating my own Attribute Set by deriving from the `UAttributeSet` class and defining my attributes shown above. I create and store an instance of it inside my `APlayerState` class which allows my `AttributeSet` to be replicated properly.

Inside the `UAttributeSet`, Unreal provides three important functions you can override:
```cpp
virtual void PreAttributeChange(const FGameplayAttribute &Attribute, float &NewValue) override;

virtual void PostAttributeChange(const FGameplayAttribute &Attribute, float OldValue, float NewValue) override;

virtual void PostGameplayEffectExecute(const struct FGameplayEffectModCallbackData &Data) override;
```

As the names suggest:
- `PreAttributeChange()` is called **before** an attribute is modified.
- `PostAttributeChange()` is called **after** an attribute's value has changed.
- `PostGameplayEffectExecute()` is triggered after a `GameplayEffect` is excuted.

These are perfect entry points to implement attributes regarding logic such as clamping attribute values, check for criticle conditions like health reaching zero, and other stuff.

In my current initial setup, I’m not using `PostAttributeChange()` yet. The `PreAttributeChange()` function is currently only used to clamp health and mana values.

On the other hand, I’m already using `PostGameplayEffectExecute()` to implement several important tasks:

- Clamping health and mana again
- Substracting the incomming damage from health
- Checking if the actor dies
- Gives the player a "HitReact" `FGameplayTag` if he got damaged
- Triggering an event with the amount of damage dealt (used by the UI to display damage numbers)
- Adding incoming experience points and triggering a level-up when a threshold is reached

There’s quite a bit happening in this function, so I’ll likely refactor it later to avoid bloating. For now, though, it works well enough.

##### Meta Attribute
Unreal's `UAttributeSet` lso supports `Meta Attributes`. These act like temporary attributes and are commonly used to manipulate other attributes. A common use case (which I’m using as well) is a `Damage` meta attribute.. Instead of directly modifying the `Health` attribute through a `GameplayEffect`, it's often better to apply changes to a meta attribute like `Damage` beforehand. This allows the damage value to be influenced by other attributes or active `GameplayEffects` - for example, buffs or debufs - before it actually affects the character’s `Health`.

If you’ve played a game like `Path of Exile`, you’ll know how complex and layered damage calculations can get. We don’t want to handle all of that complexity directly when modifying health. Instead, we want the final, resolved value after all calculations to be subtracted from `Health`.

`Meta Attributes` are perfect for this. Later in a separate devlog, I’ll show how I use Unreal’s `UGameplayEffectExecutionCalculation` to handle these complex layered damage calculations in a separate.

##### Attribute Initialization

As recommended by Epic, I initialize my starting attributes using instant `GameplayEffects` - specifically, three separate ones for Primary, Secondary, and Vital attributes.

Each of these `GameplayEffects` contains a modifier for the relevant attributes. For the Primary Attributes, I’m using scalable float values as modifiers. However, for the Secondary and Vital Attributes, the `Modifier Type` is set to `Custom Calculation Class.` This means the attribute values are calculated at runtime by a custom C++ class instead of using a fixed value. 
To be able to select a class for my attribute calculation, I created a class derived from **UGameplayModMagnitudeCalculation (ModMagCalc or MMC)**. The key advantage of using a `ModMagCalc` is that it allows me to capture any other attributes from the source and/or target `AbilitySystemComponent` and include them in the output calculation. 

Here’s an example of how I calculate the `MaxHealth` attribute based on several primary attributes:

```cpp
float UModMagCalc_MaxHealth::CalculateBaseMagnitude_Implementation(const FGameplayEffectSpec &Spec) const
{ 

    UAbilitySystemComponent *SourceASC = Spec.GetContext().GetOriginalInstigatorAbilitySystemComponent();
    if (!SourceASC)
        return 0.f;

    // Retrieve relevant attributes
    FAggregatorEvaluateParameters EvaluationParams;

    float Constitution = 0;
    float Strength = 0;
    float Dexterity = 0;
    float Intelligence = 0;

    GetCapturedAttributeMagnitude(ConstitutionDef, Spec, EvaluationParams, Constitution);
    GetCapturedAttributeMagnitude(StrengthDef, Spec, EvaluationParams, Strength);
    GetCapturedAttributeMagnitude(DexterityDef, Spec, EvaluationParams, Dexterity);
    GetCapturedAttributeMagnitude(IntelligenceDef, Spec, EvaluationParams, Intelligence);


    float MaxHealth = (Constitution * 50.0f) + (Strength * 10.0f) + ((Intelligence + Dexterity) * 5);

    return MaxHealth;
}

```

By using `GetCapturedAttributeMagnitude()`, I can capture values from other attributes defined in my attribute set. I then use those values to compute a final result, in this case, the player's MaxHealth.

I'm still undecided on whether I should create a separate class for every attribute that depends on others. I’ll need to do more research to determine whether this is a good long-term approach or if there’s a more efficient alternative. For now, I’ve only implemented the `MaxHealth` `ModMagCalc`, as this attribute suites well for further testing in the future. 