+++
date = '2024-06-10T18:47:54+02:00'
title = '#2 Project Introduction'
author = 'David'
+++

As you may have read in the last devlog, this game is a showcase project. My goal is to expand and demonstrate my skills in game development.

With that goal in mind, I’ve decided not to create a fully-fledged game but rather a vertical slice of what could potentially be a complete game. As a solo developer, delivering a large amount of high-quality content is simply not feasible, especially since I’m also working full-time. For this reason, I’m focusing on the quality of the game, but I’ll need to limit the content, length, and complexity. The result will be more of a "vertical slice"—a "demo" with around 15 minutes of gameplay.

The game itself will be a 3D top-down ARPG with Souls-like combat. The closest comparison would be No Rest for the Wicked. I chose this genre for various reasons, which I’ll explain further.

##### Why an RPG? Isn’t that too ambitious for a beginner?

Yes, it is. But I wouldn't consider myself as an beginner anymore. I’ve been learning about various aspects of game development for several years now, and even longer in software development. Still, creating an RPG is ambitious, and I spent a lot of time considering whether I wanted to take on this challenge.

On the other hand, an RPG is an excellent choice for me to challinging myself and showcasing my knowledge.
An RPG includes many systems that, on their own, can become quite complex. These systems don’t operate in isolation but instead interact on many levels.

Take the attribute system, for example. This alone could already become intricate, but it still sounds manageable—it’s just a few numbers used to calculate stuff, right? But this attribute system is not set and done. It depens on multiple things and multiple things depens on it. Maybe the damage is calculated based on your attributes. Equipment you wear or usable items you use are changing these value and need to be considered asswell. What about passive abilities or active ones that interact with these attributes for a short duration. Does the enemy character also have attributes, are these changed by equipment or levels aswell? 

So there are many parts that interact in a RPG. It is very easy to mess some number up and the whole game is imbalanced or feels off. 
And these are just the mechanics of the a RPG game. What about the NPCs, the quest system and the progress in the storyline? 

This is a huge challenge, but it’s one I’m consciously choosing to take on. I want to build a functional and scalable foundation that could support a full RPG. However, my focus are these mechanics while minimizing the content and workload in other areas I am not skilled at like art, animation, effects, story, sound and so on. I really want to deliver a high-quality game and for that I need to reduce the content of this game.

##### Why Top-Down?

I’d love to develop a third-person RPG like Gothic, but I consciously decided against it. In a third-person perspective, animations play a crucial role. Poor or mismatched animations are immediately noticeable and make the game feel unfinished. Since I neither have the skills nor the resources for high-quality animations, I opted for a top-down perspective instead.

While animations are still important in a top-down view, it’s easier to mask imperfections. Cleverly placed effects and smart design choices can help cover limitations and maintain the game quality and feel. There is a fantastic GDC talk which talking about that: [Dreamscaper: Killer Combat on an Indie Budget](https://www.youtube.com/watch?v=3Omb5exWpd4&t=3274s&ab_channel=GDC2025)

##### Why 3D?

A 2D game would undoubtedly be much simpler, and all the various RPG systems could still be implemented in a 2D game. It would save me a lot of effort in areas like animations and enviroment design. Many gameplay challenges are also much easier to solve in a 2D space.

However, 3D feels like the industry standard in game development. Since it’s highly likely that I’ll work on a 3D game in a future job, I want to prepare myself by working on a 3D project now. On the other hand, I simply love working in 3D

##### So, What’s the Game Actually About?

Game design is tough. I have several ideas that I’d love to incorporate into the game, but clearly outlining and designing them is not simple. I’m finding it quite difficult to pin down ideas at the moment. Probably, much of the design process will happen during the development. For now, I want to focus on building the foundation of a very classic RPG like character attributes, leveling systems, inventory management, and so on. Whether these elements will stay traditional or evolve into something different is something I’ll figure out along the way.

What I can say at this stage is that there won’t be a traditional class system. Instead, your playstyle will be determined by your weapon. For example, equipping a bow or a magic staff will change your attacks and abilities, similar to Elden Ring. But this concept isn’t fully fleshed out yet.

In the upcoming devlogs, I’ll start with Unreal Engine and work on implementing the core RPG elements. I want to document my progress and solutions in a way that any reader could implement them on their own. Maybe, this will help others with their own projects.