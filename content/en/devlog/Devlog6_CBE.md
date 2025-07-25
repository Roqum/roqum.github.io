+++
date = '2024-06-10T18:47:54+02:00'
title = '#6 Some small quality features (Chained by Eternity)'
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

At this point, I had some solid systems in place. Of course, they’re far from finished, but I wanted to make sure I had a strong foundation that I’m happy to build upon. Otherwise, technical debt can easily pile up and turn working on the game into a painful experience later on.

In this chapter, I’ll walk you through some small quality-of-life features and visual touches I added to the game.

### Highlighting Enemies
I wanted to give the player a visual feedback on which enemy he is currently targeting. To do this, I integrated an enemy highlighting function to my coursor trace logic inside my custom `APlayerController`.

The logic behind this feature is actually quite simple, since I’m already performing a trace each frame to determine the mouse world position. I extended this logic by checking whether the hit actor implements my custom `ICombatInterface`.

If it does, I call a `HighlightActor()` function on that interface. To be able to diable the highlighting, I cache a pointer to the previously highlighted actor. If a newly hit actor is different from the cached one, I call a function to disable the highlight on the previous actor before highlighting the new one.

<img src="/images/devlog6_CBE/HighligthingEnemies.gif" alt="Highligthing" style="display: block; width: 65%; margin: 0 auto;">

To achieve the actual highlighting effect, I added a `PostProcessVolume` to my level and assigned a custom post-process material to it. I didn’t create the material by myself, I found one online and edited it to my needs.

### Damage Numbers

I also wanted to display damage numbers when an enemy is hit. This feature tends to divide players - some love the instant feedback and to get a feeling for theire DPS, while others feel it clutters the screen. To accommodate both preferences, I made it configurable in the game settings. Players will be able to toggle damage numbers on or off based on their personal taste.

Since I already implemented my damage calculation class, I added an event trigger directly within it. For now, this event simply broadcasts the raw damage value. Later on, I’ll expand it to broadcast a struct containing more detailed information, such as whether the hit was a critical, blocked, or evaded, etc.

I also intend to include the location of the hit enemy in the event.  This allows me to animate the damage numbers away from the player, creating the illusion that the numbers are “knocked out” of them when struck. It’s a small touch, but it really enhances the impact of each hit.

<img src="/images/devlog6_CBE/DamageNumbers
.gif" alt="Highligthing" style="display: block; width: 65%; margin: 0 auto;">