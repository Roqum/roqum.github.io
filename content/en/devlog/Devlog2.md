+++
date = '2024-06-10T18:47:54+02:00'
title = '#3 Flexible Input System (Chained by Eternity)'
author = 'David'
draft = true
+++
Project of the devlog: [Chained by Eternity](https://www.david-burgstaller.de/project/chainedbyeternity/)

One key conept of a RPG is to have a flexible and customable input system. The player should be able to decide which key he want to press to activate certain abilities or items.

So keeping that in mind I wanted to start my project by implementing my input system.

### Input System 

As I am using the **Gameplay Ability System (GAS)** which is decoupling gameplay abilities very well, a great chunk of this flexible input system is allready solved for me. GAS allows me to assign certain gameplay abilities (`UGameplayAbility`) to a player and activate a assigned ability by a simple tag (`FGameplayTag`). 

Actually, by default GAS uses an `EAbilityInputID` enum which values can be assign to a gameplay ability. This ability can then by activated by its ID.

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
While this default approach works, it’s tightly coupled to code and scales bad.
Instead of that ID am using `FGameplayTag` as they are more flexible and allready well integrated within GAS.In addition to that I want to make use of the the **Enhanced Input System (EIS)** as it needs manual integration to work with GAS. 

To achieve this, I implemented a custom `UGameplayAbility` class that stores an input tag (`FGameplayTag`). This tag will be used by the `Ability System Component` to activate this ability by its tag. This class will be our base class of all our future gameplay abilities so that each ability we create can have a input tag assigned.

The Enhanced Input System setup in classical way, I created a `Mapping Context` and several `Input Actions`. This setup is allready well documented by epic [here](https://dev.epicgames.com/documentation/en-us/unreal-engine/enhanced-input-in-unreal-engine).

To link this setup with GAS I firstly created a custom `UDataAsset` links each `Input Action` with a `FGameplayTag`.

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

In addition to that I created a custom input component class that derives from `UEnhancedInputComponent`. This class is just implementing following template fucntion:

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
Before I explain whats happening here, lets look at my custom `APlayerController` class where I take use of that function:

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

}
void ADefaultPlayerController::AbilityInputTagReleased(const FInputActionValue &InputAction, FGameplayTag InputTag)
{
    
}
void ADefaultPlayerController::AbilityInputTagHeld(const FInputActionValue &InputAction, FGameplayTag InputTag)
{
    
}
```

As you can see I am overriding the `SetupInputComponent()` to bind all input actions inside my `InputConfig` Data Asset. By using my custom `BindAbilityActions` template function I can pass in three function references to handle the input pressed, input released and input held case. In addition to that the corresponding gameplay tag to the input action is passed to the handler functions.

Inside the handler function I can handle my input inside my player controler. In addition to that I can pass forward the gameplay tag to my ability system component where I can handle ability activation, retriggering or passing through input to active abilities:


```cpp
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
Now, using the Mapping Context, I can bind any key to an input action such as “Activate Ability 1.” This input action activates any ability assigned the corresponding GameplayTag, enabling flexible, player-customizable controls.





### Character Control

In addition to a classical mouse control, I aimed for implementing a optional WASD control as well, because in Souls-like combat, mouse control can feel too clunky. I personally prefer the WASD control but I know some player dislike it. So I decided to give the player the option to chance the control setting between these two in the option menu.


 


I chose Unreal Engine because I’m a C++ programmer, and there are plenty of resources available for UE development online. After some research, I decided to use the Gameplay Ability System developed by Epic Games. Originally created in conjunction with Paragon, it serves as a framework for developing multiplayer RPGs. The framework is both powerful and flexible—it’s not limited to RPGs. For example, it’s also used in the development of Fortnite.

### Character Movement

After setting up the project, I added a placeholder character and implemented basic movement controls. This included simple WASD movement as well as auto-run functionality toward mouse clicks, similar to *Diablo*. For this, I used the implementation from Unreal Engine's top-down template.

While this approach has some flaws - it requires a Nav Mesh and isn’t multiplayer compatible  - I decided to stick with it initially to get things up and running.

### Attribute Set

After setting up the movement, I started working on an Attribute Set. The Attribute Set is part of the Gameplay Ability System (GAS) and manages each character's attributes.

For now, I’ve divided my attributes into three categories: Vital Attributes, Primary Attributes, and Secondary Attributes.

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
- Critical Chance
- Critical Damage
</div>
<div>

**Vital Attributes**
- Health
- Mana
</div>

</div>

These attributes are initialized sequentially from left to right at the start of the game. Because the secondary attributes depend on the primary attributes, and the vital attributes depend on the secondary and primary ones.

These attributes are just constant numbers for now. I’ll use solely the health attribute until the game reaches a stage where it makes sense to introduce additional attributes.
