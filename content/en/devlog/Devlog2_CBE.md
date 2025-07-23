+++
date = '2024-06-10T18:47:54+02:00'
title = '#2 Flexible Input System (Chained by Eternity)'
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
One important feature of an RPG is a flexible and customizable input system. Players should be able to decide which keys they want to use for activating abilities or items.

So keeping that in mind, I started my project by trying to implementing a modular input system.

### Input System 

Since I’m using the **Gameplay Ability System (GAS)** - which is already highly modular and does a great job of decoupling gameplay logic - a large part of my flexible input system is already taken care of, as long as I integrate GAS properly.

GAS allows me to assign certain gameplay abilities (`UGameplayAbility`) to a player and activate a assigned ability by a simple tag (`FGameplayTag`). 

By default GAS uses an `EAbilityInputID` enum which values can be assign to a gameplay ability. This allows each `UGameplayAbility` to be activated based on its assigned input ID:

```cpp
UENUM(BlueprintType)
enum class EAbilityInputID : uint8
{
    None        UMETA(DisplayName = "None"),
    Confirm     UMETA(DisplayName = "Confirm"),
    Cancel      UMETA(DisplayName = "Cancel"),
    Ability1    UMETA(DisplayName = "Ability1"),
    Ability2    UMETA(DisplayName = "Ability2"),
    [...]
};
```
While this approach works, it’s tightly coupled to the code doesn’t scale well. Instead, I chose to use `FGameplayTags`. Tags are more flexible, can be created and modified at runtime, and are already well integrated into GAS. 
In addition, I wanted to make use of Unreal’s **Enhanced Input System (EIS)**, which requires manual integration to work properly with GAS.

To achieve this, I implemented a custom `UGameplayAbility` base class that stores an input tag (`FGameplayTag`). This tag will be used by the `UAbilitySystemComponent` to activate the ability via its associated tag. All future gameplay abilities in the project will inherit from this base class, allowing each one to have an input tag assigned.

I set up the Enhanced Input System (EIS) in the standard way by creating a `Mapping Context` and several `Input Actions`. This process is already well documented by Epic [here](https://dev.epicgames.com/documentation/en-us/unreal-engine/enhanced-input-in-unreal-engine).

To link this setup with GAS, I firstly created a custom `UDataAsset` that connects each `InputAction` with a `FGameplayTag`.

```cpp
USTRUCT(BlueprintType)
struct FGameInputAction
{
	GENERATED_BODY()

	UPROPERTY(EditDefaultsOnly)
	const class UInputAction* InputAction = nullptr;

	UPROPERTY(EditDefaultsOnly)
	FGameplayTag InputTag = FGameplayTag();
};

UCLASS()
class RPGTOPDOWN_API UGameInputConfig : public UDataAsset
{
	GENERATED_BODY()
	
public:
	UPROPERTY(EditDefaultsOnly, BlueprintReadOnly)
	TArray<FGameInputAction> AbilityInputActions;

	const UInputAction* FindAbilityInputActionForTag(const FGameplayTag& InputTag, bool bLogNotFound = false) const;
};
```

Next, I created a custom input component class that derived from `UEnhancedInputComponent`. This class implements the following template function which we will use to bind our input actions to gameplay tag-aware handlers.

```cpp
template<class UserClass, typename PressedFuncType, typename ReleasedFuncType, typename HeldFuncType>
void UCustomInputComponent::BindAbilityActions(const TArray<FGameInputAction>& InputActions, UserClass* Object, PressedFuncType PressedFunc, ReleasedFuncType ReleasedFunc, HeldFuncType HeldFunc)
{
	for (const FGameInputAction& Action : InputActions)
	{
		if (Action.InputAction && Action.InputTag.IsValid())
		{
			if (PressedFunc)
			{
				BindAction(Action.InputAction, ETriggerEvent::Started, Object, PressedFunc, Action.InputTag);
			}
			if (ReleasedFunc)
			{
				BindAction(Action.InputAction, ETriggerEvent::Completed, Object, ReleasedFunc, Action.InputTag);
			}
			if (HeldFunc)
			{	
				BindAction(Action.InputAction, ETriggerEvent::Triggered, Object, HeldFunc, Action.InputTag);
			}
			
		}
	}
}
```
This utility function binds each `InputAction` to the provided handler functions for pressed, released, and held events. Importantly, it also passes the associated `FGameplayTag` to the handler function when the input is triggered.

In my custom `APlayerController`, I override `SetupInputComponent()` to use this helper function and bind all the input actions defined in my input config data asset:
```cpp
void ADefaultPlayerController::SetupInputComponent()
{
    Super::SetupInputComponent();

    if (UCustomInputComponent *CustomInputComponent = CastChecked<UCustomInputComponent>(InputComponent))
    {
        check(InputConfig);
        CustomInputComponent->BindAbilityActions(InputConfig->AbilityInputActions, this, &ThisClass::AbilityInputTagPressed, &ThisClass::AbilityInputTagReleased, &ThisClass::AbilityInputTagHeld);
    }
}
void ADefaultPlayerController::AbilityInputTagPressed(const FInputActionValue &InputAction, FGameplayTag InputTag)
{
    GetAbilitySystemComponent()->AbilityInputTagPressed(InputTag);
}
void ADefaultPlayerController::AbilityInputTagReleased(const FInputActionValue &InputAction, FGameplayTag InputTag)
{
    GetAbilitySystemComponent()->AbilityInputTagReleased(InputTag);
}
void ADefaultPlayerController::AbilityInputTagHeld(const FInputActionValue &InputAction, FGameplayTag InputTag)
{
    GetAbilitySystemComponent()->AbilityInputTagHeld(InputTag);
}
```

These handler functions can either manage input directly inside the PlayerController, or - as shown above - foward the input tag to the `UAbilitySystemComponent`, which handles ability activation, re-triggering, or propagating input to active abilities.

This setup gives me the flexibility I aimed for. With the Enhanced Input System, players can freely customize their keybindings, and I can easily support multiple input contexts for devices like controllers. By using input tags with gameplay abilities, I’ve decoupled the input layer from the ability system — allowing players to assign any ability to any slot and trigger it with their preferred keys. 

### Character Control

This chapter went a lot more technical than I initially planned, I’ll try not to dive too deep into implementation details in the future. It's just a bit too time-consuming.

In addition to classic **mouse-click movement**, I wanted to support optional **WASD control**. In Souls-like combat, mouse-based movement can feel clunky and imprecise. Because of that, I personally prefer WASD control for this kind of gameplay — but I know not all players feel the same. So I decided to offer both options and allow players to switch between them in the settings menu.

Since the implementation is pretty straightforward and there are tons of tutorials available online, I won’t cover it in detail here. I didn't do anything particularly unique in this part. 

For the mouse click-to-move control, I used a `USplineComponent` to handle pathfinding. The WASD simply uses input action values to rotate and move the character in the desired direction.


