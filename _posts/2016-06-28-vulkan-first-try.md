---
layout:     post
title:      "Vulkan: First try"
author:     realitix
summary:    How to try Vulkan on Linux
category: vulkan
thumbnail: cogs
---

## Introduction

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

```
$ sudo apt-get install git dh-autoreconf pkg-config bison flex python-mako libpthread-stubs0-dev x11proto-gl-dev libdrm-dev x11proto-dri2-dev x11proto-dri3-dev x11proto-present-dev libxcb1-dev libxcb-dri3-dev libxcb-present-dev libxshmfence-dev libx11-xcb-dev libxext-dev libxdamage-dev libxcb-glx0-dev libxcb-dri2-0-dev libudev-dev libexpat1-dev llvm llvm-dev libomxil-bellagio-dev libssl-dev cmake
```

And execute theses commands :

```
~$ git clone https://github.com/mesa3d/mesa.git
~$ cd mesa
~/mesa$ ./autogen.sh
~/mesa$ ./configure --with-vulkan-drivers=intel
~/mesa$ make
```

**Warning: Do not run `sudo make install` !**

We are now going to put the driver in a separate folder :

```
~$ mkdir icd
~$ mv mesa/lib/libvulkan_intel.so icd/
~$ mv mesa/src/intel/vulkan/intel_icd.json icd/
```

You can edit the file `icd/intel_icd.json` to set the absolute path of `libvulkan_intel.so`.
Great! We have our driver, next the SDK.

## Get the SDK

We are going to download the SDK and compile it.

```
~$ git clone https://github.com/KhronosGroup/Vulkan-LoaderAndValidationLayers.git
~$ cd Vulkan-LoaderAndValidationLayers/
~/Vulkan-LoaderAndValidationLayers$ ./update_external_sources.sh
~/Vulkan-LoaderAndValidationLayers$ cmake -H. -Bdbuild -DCMAKE_BUILD_TYPE=Debug
~/Vulkan-LoaderAndValidationLayers$ cd dbuild/
~/Vulkan-LoaderAndValidationLayers/dbuild$ make
```

Let's put the SDK and driver in the path:

```
~$ export LD_LIBRARY_PATH=/home/user/Vulkan-LoaderAndValidationLayers/dbuild/loader
~$ export VK_LAYER_PATH=/home/user/Vulkan-LoaderAndValidationLayers/dbuild/layers
~$ export VK_ICD_FILENAMES=/home/user/icd/intel_icd.json
```

Everything should be good now, you can test vulkan with demo in `Vulkan-LoaderAndValidationLayers/dbuild/demos`
