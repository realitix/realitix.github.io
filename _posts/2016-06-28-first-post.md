---
layout:     post
title:      "Vulkan: First try"
author:     realitix
summary:    How to try Vulkan on Linux
category: vulkan
thumbnail: cogs
---

## Welcome, this is my first post!

In this post, I'm going to show you how to try Vulkan. I will not explain to
you what is Vulkan, you are probably here because you know it and you want to
try it.

Because I like free software, I'm going to use an Intel iGPU. You need at least
an Haswell iGPU to get Vulkan working.

## First, how it works ?

1. The application loads the Vulkan Runtime Library. We can call it Vulkan SDK
or Loader.
2. The Vulkan loader searches on the computer for Vulkan drivers.
Drivers are called ICD.
3. ICD talks to the graphic card.

So we need the loader and the ICD.

## Get the ICD

You need to compile the ICD driver because it's not released yet. Intel writes
free drivers into the Mesa3D project. Vulkan supports is added in version 12
which is not released, that's why we have to compile it.

Clone the Mesa3D repo: `https://github.com/mesa3d/mesa.git` and execute
`autogen.sh` to create the `configure.sh` file. You will get a lot of
errors, don't be scared it's because you need to install packages.
Search on internet an fix all errors. When it's done, run `make`.

**Warning: Do not run `sudo make install` !**


