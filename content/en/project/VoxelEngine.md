+++
date = '2025-07-03T15:07:36+02:00'
draft = false
title = 'Voxel Renderer / Engine'
categories = ['Vulkan', 'C++', 'SDL']
projectDescription = ["An ongoing side project to explore graphics and engine programming with Vulkan. The goal is to render a voxel world with a focus on performance and visual quality optimization."]
+++

<br>
<div style="display: grid; grid-template-columns: 150px 1fr; ">
  <div>Status:</div>
  <div>Currently in progress... </div>
  <div>Started:</div>
  <div>26. April 2025</div>
  <div>Working Hours:</div>
  <div>~30h</div>
  <div>Source Code:</div>
  <a href="https://github.com/Roqum/VoxelGameEngine" style="color:rgb(158, 166, 174); text-decoration: underline;">github.com/Roqum/VoxelGameEngine</a>
</div>
<br>

## Introduction
I’m deeply interested in game engine programming and am seriously considering transitioning fully from game development to engine development. I love the combination of solving math-heavy challenges, writing performance-critical code, and designing thoughtful systems. That’s what led me to start this project.

This voxel engine serves as my learning environment. My goal is to build a simple voxel world using procedural generation and to focus on optimizing the rendering pipeline as much as possible. There are many algorithms I’ve read about and am eager to experiment with, combine, and refine. At the end, I want to gain a deep understanding of how things work under the hood in modern game engines.

## Project Goals & Milestones

To be more specific about my goals these are some milestones which I want to achieve with this project for now. I might add stuff in the future but for now I want to get a solid Voxel rendering. My ultimate goal is to render water - because it’s a difficult and its awesome.
##### Rendering Milestones
<ul style="list-style: disc; padding-left: 20px;">
  <li><span style="display: inline-flex; width: 12px; height: 12px; border: 1px solid #aaa; background-color: white; color: green; font-size: 16px; font-weight: 900; justify-content: center; align-items: center; margin-right: 8px; line-height: 1; vertical-align: middle; position: relative; top: -1px;">✓</span>
  Set up Vulkan rendering pipeline
  </li>
  <li><span style="display: inline-flex; width: 12px; height: 12px; border: 1px solid #aaa; background-color: white; color: green; font-size: 16px; font-weight: 900; justify-content: center; align-items: center; margin-right: 8px; line-height: 1; vertical-align: middle; position: relative; top: -1px;">✓</span> 
  Render a voxel cube
  </li>
  <li><span style="display: inline-flex; width: 12px; height: 12px; border: 1px solid #aaa; background-color: white; color: green; font-size: 16px; font-weight: 900; justify-content: center; align-items: center; margin-right: 8px; line-height: 1; vertical-align: middle; position: relative; top: -1px;">✓</span> 
  Render a voxel chunk
  </li>
  <li><span style="display: inline-block; width: 12px; height: 12px; border: 1px solid #aaa; background-color: white; margin-right: 8px; vertical-align: middle; position: relative; top: -1px;"></span>
   Render a voxel world (procedurally generated)
   </li>
  <li><span style="display: inline-block; width: 12px; height: 12px; border: 1px solid #aaa; background-color: white; margin-right: 8px; vertical-align: middle; position: relative; top: -1px;"></span>
   Implement basic lighting
   </li>
  <li><span style="display: inline-block; width: 12px; height: 12px; border: 1px solid #aaa; background-color: white; margin-right: 8px; vertical-align: middle; position: relative; top: -1px;"></span>
   Add global illumination
   </li>
  <li><span style="display: inline-block; width: 12px; height: 12px; border: 1px solid #aaa; background-color: white; margin-right: 8px; vertical-align: middle; position: relative; top: -1px;"></span>
   Rendering water
   </li>
</ul>

##### Rendering Optimization
<ul style="list-style: disc; padding-left: 20px;">
  <li><span style="display: inline-flex; width: 12px; height: 12px; border: 1px solid #aaa; background-color: white; color: green; font-size: 16px; font-weight: 900; justify-content: center; align-items: center; margin-right: 8px; line-height: 1; vertical-align: middle; position: relative; top: -1px;">✓</span> Implement face culling</li>
  <li><span style="display: inline-block; width: 12px; height: 12px; border: 1px solid #aaa; background-color: white; margin-right: 8px; vertical-align: middle; position: relative; top: -1px;"></span> Implement greedy meshing</li>
  <li><span style="display: inline-block; width: 12px; height: 12px; border: 1px solid #aaa; background-color: white; margin-right: 8px; vertical-align: middle; position: relative; top: -1px;"></span> Try out binary meshing</li>
  <li><span style="display: inline-block; width: 12px; height: 12px; border: 1px solid #aaa; background-color: white; margin-right: 8px; vertical-align: middle; position: relative; top: -1px;"></span> Implement Dynamic Chunk loading</li>
  <li><span style="display: inline-block; width: 12px; height: 12px; border: 1px solid #aaa; background-color: white; margin-right: 8px; vertical-align: middle; position: relative; top: -1px;"></span> octree-based chunk management (Not sure yet)</li>
</ul>

##### Memory Optimization
<ul style="list-style: disc; padding-left: 20px;">
  <li><span style="display: inline-block; width: 12px; height: 12px; border: 1px solid #aaa; background-color: white; margin-right: 8px; vertical-align: middle; position: relative; top: -1px;"></span> Improve voxel memory allocation</li>
</ul>

## Current Setup

First step was to implement a functional render pipeline to get Vulkan working. So there was a lot to learn about Vulkan initialization and a lot of boiler plate coding to do. Following the [vulkan-tutorial.com](https://vulkan-tutorial.com/) tutorial I got a grasp of how stuff is running pretty fast and could make some refinement alongway.

- **Window and surface** using GLFW  
- **Swapchain** handling double-buffered presentation
- **Render pass and framebuffers**
- **Graphics pipeline** with custom vertex and fragment shaders 
- **Vertex and index buffers** for mesh data  
- **Staging buffer** using a dedicated transfer queue family (if available)  
- **Uniform buffer** for camera perspective and mesh rotation 
- **Depth buffer** for proper 3D depth rendering  
- **Texture loading** using `stb_image`
- **Command buffers and command pool** for recording rendering commands  
- **CPU-GPU Synchronization** using semaphores and fences

## Challanges I Faced

With a working rendering pipeline as a foundation, I finally got to the fun part: rendering my first voxel - a simple cube with 6 faces and 12 triangles.

That should be easy, I thought...

But I quickly realized something was wrong. The cube I imagined in my head didn't match what appeared on screen. I assumed my coordinates were off, so I kept tweaking values - even drew it on paper - but still couldn’t get it right. Eventually, through trial and error, I got a cube that looked correct.
Since I couldn’t understand why those coordinates worked, I felt pretty dumb. Maybe I had just mixed up the coordinate axes, I thought. So I decided to test that by rendering a 16×16 voxel chunk next.

But the results were even more confusing. No matter what vertices or indices I used, it looked totally broken:

<img src="/images/VoxelEngine/IndiciesInt16Bug.gif" alt="Buggy Chunk" style="display: block; width: 40%; margin: 0 auto;">

At that point, I knew something in my rendering pipeline was off, so I took a step back and started reviewing my setup.
After hours of debugging, searching, and testing, I finally found the issue:

```cpp
vkCmdBindIndexBuffer(commandBuffer, vkIndexBuffer, 0, VK_INDEX_TYPE_UINT16);
```
This line binds the index buffer - and it looked fine at first. But I had defined my indices like this:

```cpp
	std::vector<uint32_t> indices;
```
Spot the issue? I told Vulkan to interpret the buffer as 16-bit indices, but actually gave it 32-bit data. That caused incorrect memory alignment and unpredictable rendering behavior.

Once I fixed it by switching to `VK_INDEX_TYPE_UINT32`, everything rendered correctly - and suddenly the cube coordinates I had in my mind made a lot more sense!

<img src="/images/VoxelEngine/16x16ChunkRendering.gif" alt="Buggy Chunk" style="display: block; width: 65%; margin: 0 auto;">


This experience reminded me of the harsh reality of low-level graphics programming: no error messages, no warnings, just a broken rendering image.

But honestly, I love it.

Sure, it's frustrating to spend hours tracking down a small bug. But finally solving it is incredibly rewarding. And especially in graphics programming, creating new something new out of pure code feels amazing. That's why I love programming and especially game development.

## What comes next?

Firstly, I need to refactor the mess. I just wanted to get stuff working and didn't bother about organizing my code. But now it’s time to clean it up. Otherwise, continuing development will become a pain later on.

After that I want to  implement an algorithm using perlin noise to generate a voxel world terrain to render. 

And finally, if that is works, I'm going to focus on optimizing my rendering so I can make the world huge without performance losses.

These are my next steps, but it may take a while, since this is just my side project and I need to stay focused on my main project: a top down ARPG, which you can find [here](https://www.david-burgstaller.de/project/chainedbyeternity/).
