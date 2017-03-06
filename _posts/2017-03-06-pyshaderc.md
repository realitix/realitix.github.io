---
layout:     post
title:      "PyShaderc: Compile to Spir-V like a boss"
author:     realitix
summary:    Glsl to Spir-V so easy
category:   vulkan
thumbnail:  hand-peace-o
---

## Introduction

Like you know, [Vulkan](https://www.khronos.org/vulkan/) needs to be fed with Spir-V shaders. To do so, you will write your shader in glsl and you will use [shaderc](https://github.com/google/shaderc) or [glslang](https://github.com/KhronosGroup/glslang) to compile your Glsl code into Spir-V (shaderc uses glslang internally).

Currently, you must use the command line or C/C++ code to compile your shaders.

## PyShaderc

**PyShaderc** is a wrapper around shaderc (using cffi internally) that gives you the ability to compile your shader to Spir-V in only one line of Python!

### Let's take a look

#### Install it

```python
pip install pyshaderc
```

#### Run it

```python
import pyshaderc
spirv = pyshaderc.compile_file_into_spirv('/tmp/myshader.vs.glsl', 'vert')
```

The variable `spirv` contains your Spir-V code! Another module to add into [vulk](https://github.com/realitix/vulk). This allows you to work only with Glsl code, like with OpenGL.

More informations on Github: [PyShaderc](https://github.com/realitix/pyshaderc)
