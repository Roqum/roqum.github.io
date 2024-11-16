+++
date = '2024-06-10T18:47:54+02:00'
title = 'Devlog 3: First Steps'
author = 'David'
+++

### Gameplay Ability System

I chose Unreal Engine because I’m a C++ programmer, and there are plenty of resources available for UE development online. After some research, I decided to use the Gameplay Ability System developed by Epic Games. Originally created in conjunction with Paragon, it serves as a framework for developing multiplayer RPGs. The framework is both powerful and flexible—it’s not limited to RPGs. For example, it’s also used in the development of Fortnite.

If you want to learn more about the Gameplay Ability System (GAS), feel free to check out [my documentation](https://david-burgstaller.de/documentation). I can also highly recommend [tranek’s documentation](https://github.com/tranek/GASDocumentation?tab=readme-ov-file#intro) on the subject. Additionally, the GAS framework is well-documented within its own source code.
As I continue my development journey, I aim to expand my own GAS documentation. It helps me better understand this extensive framework and organize what I’ve learned.

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

Checkout [my documentation](https://david-burgstaller.de/documentation/gameplayabilitysystem) for implementation details.