+++
draft = true
date = '2025-06-21T18:47:54+02:00'
title = 'Niagara System: Export Particle Data to C++/Blueprint'
author = 'David'
topic = 'Unreal Engine'
+++

## Overview

In a Niagara System you able to create events which are normaly bind to inside the System. But Unreal Engine offers you a have to bind to Niagara System Events even outside of the system.

But it should be said that its a not a good practive to mix VFX and gameplay logic, espacially in multiplayer games since VFX runs almost client side only. So use this with caution.

## Blueprint Setup

To be able to receive the particle export data of a niagara system you need to implement the `NiagaraParticleCallbackHandler` Interface

## Creating C++ Receiver Class

To be able to receive the particle export data of a niagara system you need to implement the EXportDataInterface

## Creating the Niagara System

The Niagara System has to be set to CPU , otherwise it wont work. If you are running a GPU Niagara System thatn this wont work for you. 

To export particle data you have to add the export data event to the script.