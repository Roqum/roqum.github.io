+++
title = 'Chained By Eternity (Main Project)'
date = '2025-06-26T12:39:17+02:00'
draft = false
categories = ['Unreal Engine 5', 'Gameplay Ability System', 'C++']
projectDescription = ["A vertical slice of a classic action RPG, built in Unreal Engine 5 using the Gameplay Ability System. Started in March 2024 as a long-term project. It showcases all core mechanices of a classic RPG like real-time combat, character progression, ability system and much more."]
+++

## Project Info

<br>
<div style="display: grid; grid-template-columns: 150px 1fr; ">
  <div>Last page update:</div><div>25. Juli 2025</div>
  <div>Status:</div><div>Currently in progress...</div>
  <div>Started:</div><div>17. February 2024</div>
  <div>Teamsize:</div><div>2 Programmers</div>
  <div>My Working Hours:</div><div>~500h</div>
  <div>Source Code:</div><div>Coming soon</div>
  <div>Demo:</div><div>Coming soon</div>

</div>
<br>

> **Note:** \
> This project page is **meant purely for showcasing** the current state of the game, with lots of images and minimal explanation.
> If you're interested in implementation details, please check out my **devlogs** [here](https://david-burgstaller.de/devlog/) - they go much deeper into the technical side of development.

> **Note 2:**  
> Thanks for taking the time to read about my project.  
> I would really appreciate any kind of **feedback** - whether it’s about the project, my writing style, or even my grammar.  
> Feel free to send me an email - **it really helps!**

## Intorduction

**Chained By Eternity** is my most ambitious and dedicated project to date. My goal with this project is to showcase my skills and finally land a job in the game development industry.

Together with a friend, we set out to build something technically challenging. We knew early on that building a full-scale ARPG like *Path of Exile* or *Diablo* would be unrealistic for a two-person team. Instead, we focused on creating a **high-quality vertical slice** a playable snapshot of what a complete game could be. This includes real-time combat, a fully functional ability and progression system, an inventory and item systems loot mechanics, and many other core features typically found in a classic action RPG.


## Technical Details

This project is built using **Unreal Engine 5 (UE5)** and the **Gameplay Ability System (GAS)**. We're developing almost **entirely in C++**, with minimal use of Blueprints.

If you use Unreal Engine and don't know about the Gameplay Ability System, I highly recommend looking into it. It's a very powerful and well-designed system — and not just for big projects. That said, it can be quite complex, even though it's well documented in the codebase. You might want to start with (Epic’s official introduction)[https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-ability-system-for-unreal-engine] or have a look at (tranek’s excellent documentation)[https://github.com/tranek/GASDocumentation] on GitHub.

### Overview 
We've already achieved a lot, but there's still a long road ahead. Many systems are implemented and working, but **not yet at the level of quality we're aiming for**. We're currently focused on building a solid foundation, following the principle:

> "Make it work first, make it good later."

Please keep that in mind when reading the overview below. Most of the listed systems are **functional but still need polish** and refinement.

#### Features Already Implemented

- **Advanced Attribute System**  
  Modular and expandable attribute system with stat scaling and interactions.
- **Multiple Weapon Types**  
  Currently supports 2 weapon types (Sword, Bow). Third one (magic wand) is planned.
- **Inventory and Equipment System**  
  Allows players to manage items, equip, and unequip equipment.
- **Items**  
  Items can modify player attributes, grant new abilities when equipped or activate abilities when used.
- **Checkpoint System**  
  Basic save/load functionality to restore the player's game state.
- **Basic Enemy AI**  
  Simple behavior logic like chasing and attacking the player. (This will be completly reworked)
- **Combat Abilities**  
  Including single-target, area-of-effect (AOE), and projectile-based abilities.
- **UI**  
  Functional UI for the main menu, setting menu, in-game HUD, and inventory management.
- **Loot Mechanics**  
  For complex item drop calculation based on enemy level and rarity.
---

#### Big systestems that are missing but planned

- **Level Environment & Art**  
  Creating visually appealing level environment.
- **Enemy Spawn System**  
  Procedurally generated spawn algorithm for random enemy placement.
- **Quest System**  
  Including dialogues, progression tracking, and rewards.
- **Advanced AI**  
  Smarter enemy behavior for diffrent enemy type (Probably going for state trees).
- **Ability & Attribute Menus (UI)**  
  UI for managing abilities and attributes.
- **General Polish**  
  Animations refinement, VFX/SFX, optimize performance, etc.

# Showcase

Below is a collection of screenshots, videos, and gifs showcasing our current development progress. I’ll try to keep this section updated regularly — you can check the project info section at the top of this page to see the last update date.

##### Melee Combat
<br>
<video width="100%" controls  style="display: block; width: 90%; margin: 0 auto;">
  <source src="/videos/ChainedByEternity/CombatShowcase.mp4" type="video/mp4">
</video>

Implemented:
<ul style="margin-top: 0rem; margin-left: 2rem; font-size: 1rem; line-height: 1.6;">
  <li>Souls-like hitbox capsule activation system</li>
  <li>Accurate blood VFX based on hit location</li>
  <li>Dynamic blood decals</li>
  <li>Impact and woosh sound effects</li>
  <li>Damage numbers <em>(can be toggled in settings)</em></li>
  <li>Camera shake on hit <em>(can be toggled in settings)</em></li>
  <li>Light knockback effect on impact</li>
</ul>

##### Ranged Combat
<br>
<video width="100%" controls  style="display: block; width: 90%; margin: 0 auto;">
  <source src="/videos/ChainedByEternity/BowAttackSchowcase.mp4" type="video/mp4">
</video>

Implemented:
<ul style="margin-top: 0rem; margin-left: 2rem; font-size: 1rem; line-height: 1.6;">
  <li>Self-made projectile VFX <em>(still a work in progress)</em></li>
</ul>
In Progress:
<ul style="margin-top: 1rem; margin-left: 2rem; font-size: 1rem; line-height: 1.6;">
  <li>Projectile and impact sound effects</li>
  <li>Blood VFX on hit</li>
  <li>Projectile trail gets instantly removed on hit <em>(bug)</em></li>
</ul>
<br>

<img src="/images/ChainedByEternity/RainOfArrowDamage.gif" alt="Main Menu" style="display: block; width: 100%; margin: 0 auto;">

<ul style="margin-top: 1rem; margin-left: 2rem; font-size: 1rem; line-height: 1.6;">
  <li>Linetrace for checking enemy hit</li>
  <li>Has cooldown</li>
</ul>

##### Footstep Sound Based on Ground
<br>
<video width="100%" controls  style="display: block; width: 90%; margin: 0 auto;">
  <source src="/videos/ChainedByEternity/FootstepSoundBasedOnGroundShowcase.mp4" type="video/mp4">
</video>
<br>
<img src="/images/ChainedByEternity/AnimationFootstepByGround.png" alt="Main Menu" style="display: block; width: 100%; margin: 0 auto;">
<br>

- Custom `AnimNotify` Class. `SoundCue` for given underground can be set in the details panel. If None, default sound is played

### Inventory

##### Item Drops
<br>
<img src="/images/ChainedByEternity/ItemDropShowcase.gif" alt="Main Menu" style="display: block; width: 100%; margin: 0 auto;">
<br>
<ul style="margin-top: 1rem; margin-left: 2rem; font-size: 1rem; line-height: 1.6;">
  <li>Items that drop close to each other will stack on top of one another <em>(prevents overlapping text)</em></li>
</ul>

##### Collecting & Equipping Items
<br>

<video width="100%" controls  style="display: block; width: 90%; margin: 0 auto;">
  <source src="/videos/ChainedByEternity/InventoryShowcase.mp4" type="video/mp4">
</video>

Implemented:
<ul style="margin-top: 0rem; margin-left: 2rem; font-size: 1rem; line-height: 1.6;">
  <li>Item stats are randomized</li>
  <li>Item stats modify the character’s attributes</li>
  <li>Changing weapons updates both abilities and animation layers</li>
</ul>
In progress:
<ul style="margin-top: 1rem; margin-left: 2rem; font-size: 1rem; line-height: 1.6;">
  <li>Drag and drop</li>
  <li>Sound effects</li>
  <li>Item Sorting button</li>
  <li>UI design polish</li>
</ul>

### Main Menu
<br>
<img src="/images/ChainedByEternity/MainMenuShowcase.gif" alt="Main Menu" style="display: block; width: 100%; margin: 0 auto;">
<br>

##### Settings Menu
<br>
<div style="position: relative; width: 100%; margin: 1rem auto; overflow: hidden;">
  <div id="slider" style="display: flex; transition: transform 0.6s ease-in-out; width: 100%;">
    <img src="/images/ChainedByEternity/SettingsControls.png" alt="Main Menu" style="width: 100%; flex-shrink: 0;">
    <img src="/images/ChainedByEternity/GraphicsSettings.png" alt="Main Menu" style="width: 100%; flex-shrink: 0;">
    <img src="/images/ChainedByEternity/AudioSettings.png" alt="Main Menu" style="width: 100%; flex-shrink: 0;">
    <img src="/images/ChainedByEternity/GameplaySettings.png" alt="Main Menu" style="width: 100%; flex-shrink: 0;">
    <img src="/images/ChainedByEternity/InterfaceSettings.png" alt="Main Menu" style="width: 100%; flex-shrink: 0;">
  </div>

   <!-- Left Arrow -->
  <button onclick="moveSlide(-1)" style="position: absolute; top: 50%; left: 10px; transform: translateY(-50%); font-size: 2rem; background-color: rgba(0,0,0,0.5); color: white; border: none; cursor: pointer; padding: 5px 10px;">‹</button>
  <!-- Right Arrow -->
  <button onclick="moveSlide(1)" style="position: absolute; top: 50%; right: 10px; transform: translateY(-50%); font-size: 2rem; background-color: rgba(0,0,0,0.5); color: white; border: none; cursor: pointer; padding: 5px 10px;">›</button>
</div>

<br>
<img src="/images/ChainedByEternity/GraphicSettings.gif" alt="Main Menu" style="display: block; width: 100%; margin: 0 auto;">
<br>

<script>
  let currentIndex = 0;
  const slider = document.getElementById("slider");
  const totalSlides = slider.children.length;

  function moveSlide(direction) {
    currentIndex += direction;
    if (currentIndex < 0) currentIndex = totalSlides - 1;
    if (currentIndex >= totalSlides) currentIndex = 0;
    slider.style.transform = `translateX(-${currentIndex * 100}%)`;
  }

  // Auto-slide every 3 seconds
    setInterval(() => {
      moveSlide(1);
    }, 3500);
</script>

##### Character Creation (Unfinished)
<br>
<img src="/images/ChainedByEternity/CharacterCreationMenu.png" alt="Main Menu" style="width: 100%; flex-shrink: 0;">

##### Loading Menu (Unfinished)
<br>
<img src="/images/ChainedByEternity/LoadingMenu.png" alt="Main Menu" style="width: 100%; flex-shrink: 0;">

##### Credits
<br>
<img src="/images/ChainedByEternity/CreditsScreen.png" alt="Main Menu" style="width: 100%; flex-shrink: 0;">

