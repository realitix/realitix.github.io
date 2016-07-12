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

First, prepare your environment. On ubuntu 16.04, install theses packages :

'''
$ sudo apt-get install git dh-autoreconf pkg-config bison flex python-mako libpthread-stubs0-dev x11proto-gl-dev libdrm-dev x11proto-dri2-dev x11proto-dri3-dev x11proto-present-dev libxcb1-dev libxcb-dri3-dev libxcb-present-dev libxshmfence-dev libx11-xcb-dev libxext-dev libxdamage-dev libxcb-glx0-dev libxcb-dri2-0-dev libudev-dev libexpat1-dev llvm
'''

And execute theses commands :

'''
$ git clone https://github.com/mesa3d/mesa.git
$ cd mesa
$ ./autogen.sh
$ ./configure --with-vulkan-drivers
$ make
'''

**Warning: Do not run `sudo make install` !**


