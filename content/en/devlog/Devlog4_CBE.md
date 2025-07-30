+++
date = '2024-04-11T18:47:54+02:00'
title = '#4 First Basic Combat (Chained by Eternity)'
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

Now that I have an attribute system in place, it's time to test it - and what better way to do that than by implementing basic combat?
I decided to dive into `GameplayAbilities` and implement my first combat ability to deal damage to a target.

### Damage Gameplay Abilities

For my damage based `GameplayAbilities`, I created 
a custom `UDamageGameplayAbility` class to handle all damage-related logic.

##### Damage Types

First of all this class defines a public `DamageTypes` map like this:

```cpp
UPROPERTY(EditDefaultsOnly, BlueprintReadOnly, Category = "Damage")
	TMap<FGameplayTag, FScalableFloat> DamageTypes;

```

This allowes me to assign multiple damage types with corresponding damage values to this ability - either via Blueprint or in C++.  
Each damage type is represented by a `FGameplayTag`. So far, I've defined four basic types:
- Physical Damage
- Fire Damage
- Ice Damage
- Arcane Damage

I’m not yet sure whether I’ll keep all of these, but changing or expanding them later on shouldn’t be an issue at all.

##### Damage Calculation

Secondly, I define a `UGameplayEffect` variable that tells the ability which `GameplayEffect` class to use for applying damage. The `GameplayEffect` itself handles how the `Ability System Component` applies damage to the specified target.

```cpp
UPROPERTY(EditDefaultsOnly, BlueprintReadOnly)
TSubclassOf<UGameplayEffect> DamageEffectClass;
```

In addition, I implemented an `ApplyDamageToTarget(AActor* TargetCharacter)` function. This utility function can be called by any future `GameplayAbilitiy` that derives from this class to apply damage to a specified target.

```cpp
void UDamageGameplayAbility::ApplyDamageToTarget(AActor* TargetCharacter)
{
	if (TargetCharacter)
	{
		const UAbilitySystemComponent* SourceAbilitySystemComponent = UAbilitySystemBlueprintLibrary::GetAbilitySystemComponent(GetAvatarActorFromActorInfo());
		const FGameplayEffectSpecHandle SpecHandle = SourceAbilitySystemComponent->MakeOutgoingSpec(DamageEffectClass, GetAbilityLevel(), SourceAbilitySystemComponent->MakeEffectContext());

		for (TPair<FGameplayTag, FScalableFloat>& DamageTypePair : DamageTypes)
		{
			const float ScaledDamage = DamageTypePair.Value.GetValueAtLevel(GetAbilityLevel());
			UAbilitySystemBlueprintLibrary::AssignTagSetByCallerMagnitude(SpecHandle, DamageTypePair.Key, ScaledDamage);
		}

		UAbilitySystemComponent* TargetAbilitySystemComponent = UAbilitySystemBlueprintLibrary::GetAbilitySystemComponent(TargetCharacter);
		if (TargetAbilitySystemComponent)
		{
			TargetAbilitySystemComponent->ApplyGameplayEffectSpecToTarget(*SpecHandle.Data.Get(), TargetAbilitySystemComponent);
		}
	}
}
```
Inside this function, I iterate through all damage types set on the ability and use `AssignTagSetByCallerMagnitude` to assign these values to the corrusponding tag. These values are then passed to the `GameplayEffectSpecHandle` which is applied to the target at the end of the function.

Inside my associated `GameplayEffect` class,  I refer to these set-by-caller magnitudes. To do so, I set the `Modifier Type` to a `Custom Calculation Class`. This allows me to implement a custom `UGameplayEffectExecutionCalculation` class, where I perform all complex damage calculations.  
Here's an excerpt from that class:

```cpp
void UExecCalc_Damage::Execute_Implementation(const FGameplayEffectCustomExecutionParameters& ExecutionParams, 
	FGameplayEffectCustomExecutionOutput& OutExecutionOutput) const
{
	const UAbilitySystemComponent* SourceAbilitySystemComponent = ExecutionParams.GetSourceAbilitySystemComponent();
	const UAbilitySystemComponent* TargetAbilitySystemComponent = ExecutionParams.GetTargetAbilitySystemComponent();

    const FGameplayEffectSpec &Spec = ExecutionParams.GetOwningSpec();

    const FGameplayTagContainer *SourceTags = Spec.CapturedSourceTags.GetAggregatedTags();
    const FGameplayTagContainer *TargetTags = Spec.CapturedTargetTags.GetAggregatedTags();
    FAggregatorEvaluateParameters EvaluationParameters;
    EvaluationParameters.SourceTags = SourceTags;
    EvaluationParameters.TargetTags = TargetTags;


	float Damage = 0;
	for (FGameplayTag DamageTypeTag : FGameGameplayTags::Get().DamageTypes)
	{
		float const DamageTypeValue = Spec.GetSetByCallerMagnitude(DamageTypeTag);

		// Physical damage is reduced by armor
		if (DamageTypeTag.MatchesTagExact(FGameGameplayTags::Get().Damage_Physical))
		{
            float Armor = 0.f;
            ExecutionParams.AttemptCalculateCapturedAttributeMagnitude(DamageStatics().ArmorDef, EvaluationParameters, Armor);
            Armor = FMath::Max<float>(0.f, Armor);

            float BlockedDamagePercentage = Armor / (Armor + (10 * DamageTypeValue));
            Damage += DamageTypeValue - (BlockedDamagePercentage * DamageTypeValue);
		}

        [...]

	}

    const FGameplayModifierEvaluatedData EvaluatedData(UBasicAttributeSet::GetIncomingDMGAttribute(), EGameplayModOp::Override, Damage);
    OutExecutionOutput.AddOutputModifier(EvaluatedData);
}

```

These `UGameplayEffectExecutionCalculation` classes are extremly powerfull. Here I can incorporate multiple attributes from both the source and target `AbilitySystemComponent` into my calculations. I could even change those attributes on the fly or perform any other crazy stuff a programmer could dream of. The only disadvanted is that these classes are not predicted which my be important to know if you are working on a multiplayer game.

In my current setup, things are still fairly straightforward: I reference the tag-assigned magnitudes from earlier, account for the targets resistances like armor, and reduce the final damage accordingly.

The example above demonstrates how physical damage is reduced based on the target's armor using a mathematical formulat. Over time, I plan to expand this system to handle more combat mechanics like criticle hits, 
evasion, block chances, etc.


### Melee Attack Gameplay Ability

Okay, that was a lot of theory - I really want to finally hit something.

To get started, I needed to prepare a few animations. I'm keeping things simple for now, but later I plan to refine the melee combat system with features like **combo attacks** that depend on the equipped weapon, and an **enable/disable capsule hitbox system** similar to what you’d see in the Souls series.

But for now, I just want to keep it simple: play an attack animation, spawn a hitbox sphere, and apply damage to everything that can be hit inside it.

First, I created a attack `Animation Montage` and added a `AnimNotify` that is triggering a `GameplayEvent` using a specified `GameplayTag`. I Then added a sword mesh and created a `CombatSocket` socket on it  to define the location where the hitbox sphere will be spawned during an attack.  
On my character, I added two sockets - `WeaponLeft` and `WeaponRight` - where I can attach my weapon class later on. Next, I created a weapon `AActor` class, assigned the sword mesh to it and attached this actor to my character.

To keep the design cleaner and more scalable, I also created a `**ICombatInterface**`. My reasoning was that, later on, I might want to hit things beyond just enemy characters like breakable walls or other interactive static `AActors`. By having those actors implement the `ICombatInterface`, I can make them "combat-aware." The interface defines what needs to be implemented for an `AActor` to interact with the combat system.

There was one last thing missing from my Melee Attack `GameplayAbility`: I only want to attack if the player actually clicks on something that can be attacked. I solved this by creating a custom `AbilityTask` that performs a mouse-to-world raycast and checks if a valid, hittable `AActor` was hit.

With everything prepared, the final structure of my Melee Attack `GameplayAbility` is actually quite simple:

- When the ability is activated, I run the custom `AbilityTask` to detect whether a valid target (an actor that implements `ICombatInterface`) was hit.
- If the result is valid, I use `AbilityTask_PlayMontageAndWait` to play my attack `Animation Montage`.
- Then, I call `AbilityTask_WaitForGameplayTag` to listen for the `AnimNotify` tag embedded in the animation.
- When the notify is received, I spawn an invisible hitbox sphere at the weapon’s socket location and check for any overlapping actors that implement `ICombatInterface`
- For each valid target, I call `ApplyDamageToTarget()` as described earlier to apply the damage.

All together is looks like this:

<video width="100%" controls  style="display: block; width: 80%; margin: 0 auto;">
  <source src="/videos/devlog4_CBE/FirstCombat.mp4" type="video/mp4">
</video>

It looks pretty clunky but it works. Enemies are hit, Damage is dealt and health is lost. So we can move on for now.