+++
date = '2025-07-03T15:07:36+02:00'
draft = true
title = 'Voxel Renderer / Engine'
categories = ['Vulkan', 'C++', 'SDL']
projectDescription = ["An ongoing project to explore graphics and engine programming with Vulkan. The goal is to render a voxel world with a focus on performance and visual quality optimization."]
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

With a working rendering pipeline as fundament I was finally able to get to the fun part. Rendering my first voxel. A simple cube with 6 faces, 12 traingles. 
That should be easy, I thought....
Well, I felt very dumb because somehow my coordinates in my head that should make up a cube did not align with the results of my rendering.
It was just later when I found that the issue were not the coordinates...
but instead of logic I used try and error to get my cube rendered.

Trying to render a chunk in my next step, revelead that something is not working correctly. It did not matter what indicies and verticies I used I always got something looking like this:

<img src="/images/VoxelEngine/IndiciesInt16Bug.gif" alt="Buggy Chunk" style="display: block; width: 40%; margin: 0 auto;">

Hours of debugging, searching and testing and I finally found the issue:

Binding my index buffer I usen following code:
```cpp
vkCmdBindIndexBuffer(commandBuffer, vkIndexBuffer, 0, VK_INDEX_TYPE_UINT16);
```
Everything is fine. Thats how it should be bind
But I defined my indicies vector like this:
```cpp
	std::vector<uint32_t> indices;
```
You probably spotted the issue. I was telling Vulkan that my to work with indicies of type uint16 but defined my indicies vector of type uint32 which leads to wrong memory allocation and accessing. 

Once I changed my binding to VK_INDEX_TYPE_UINT32. Everything worked perfectly and I did not feel dumb for not getting the verticies correct anymore.

<img src="/images/VoxelEngine/16x16ChunkRendering.gif" alt="Buggy Chunk" style="display: block; width: 65%; margin: 0 auto;">


I learned the harsh reality of graphics programming. No error messages, no warning, no hint where to search. But honestly I love it. Sure its frustrating to search for a bug for hours but its so rewarding to finially fix it. To create new stuff from almost nothing feels amazing. Thats why I love programming and espacially game development.

## What comes next?

Firstly, I need to refactor this mess. I just wanted to get stuff working and did not bother about organizing my code. But now I have to clean up a little bit otherwise it will be a pain later to work on this project. 

After that I want to use some king of perlin noise algorithm to generate a voxel world terrain which I want to render. 

And finally if that is working I want to optimize my rendering so that I can make this world huge.

These are my next steps but it may take a while as this is just my side project and I need to focus on my main project a Top Down ARPG which you can find [here](https://www.david-burgstaller.de/project/ChainedByEternity).
