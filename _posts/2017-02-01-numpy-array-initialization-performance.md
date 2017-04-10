---
layout:     post
title:      "NumPy: Faster initialization"
author:     realitix
summary:    Numpy can surprise you sometime
category:   numpy
thumbnail:  flash
---

## Introduction

NumPy is the fundamental package for scientific computing with Python. It contains among other things:

- a powerful N-dimensional array object
- sophisticated (broadcasting) functions
- tools for integrating C/C++ and Fortran code
- useful linear algebra, Fourier transform, and random number capabilities

In this article, we'll focus on the initialization of NumPy arrays.


## Context

Usually, to create a NumPy array, you will use the `numpy.array` function but there is another way to do
it: `numpy.fromiter`. The main difference is that `numpy.fromiter` can take any iterable, if your class
implements `__iter__`, you can pass it to `numpy.fromiter`.

*Note, `numpy.fromiter` only work with 1-dimensional array.*

In this post, I benchmark performance between these two methods of initialization. Tests are done on
Ubuntu 16.04 with Python 3.6 and NumPy 1.11.3.

At first glance, you may think that `fromiter` is slower but you will be surprised.


## Show me the code !

Here is the code to benchmark:

```
#!/usr/bin/env python3.6
import time
import numpy as np
import pygal
from os import path

list_sizes = [2**x for x in range(1, 11)]
seconds = 60


def test(size_array):
    pyarray = [float(x) for x in range(size_array)]

    # fromiter
    start = time.time()
    iterations = 0
    while time.time() - start <= seconds:
        np.fromiter(pyarray, dtype=np.float32, count=size_array)
        iterations += 1
    fromiter_count = iterations

    # array
    start = time.time()
    iterations = 0
    while time.time() - start <= seconds:
        np.array(pyarray, dtype=np.float32)
        iterations += 1
    array_count = iterations

    return array_count, fromiter_count


results = {}

for size_array in list_sizes:
    res = test(size_array)
    results[str(size_array)] = res
    print(f"Result for size {size_array} in {seconds} seconds: {res}")

out_folder = path.dirname(path.realpath(__file__))
print(f"Create diagrams in {out_folder}")

chart = pygal.Line()
chart.title = f"Performance in {seconds} seconds"
chart.x_title = "Array size"
chart.y_title = "Iterations"
chart.x_labels = list(results.keys())
chart.add('np.array', [x[0] for x in results.values()])
chart.add('np.fromiter', [x[1] for x in results.values()])
chart.render_to_png(path.join(out_folder, f'result_{seconds}.png'))
```

To be able to execute this code, install `numpy`, `pygal` and `cairosvg` packages.
This code creates a maximum number of NumPy arrays in 60 seconds. The array size is
increased several times.


## Result

![Numpy stats](/img/numpy_init_performance.png)

On this graph, the higher the better. What you see is that `numpy.fromiter` is faster
than `numpy.array` until array of size 256.

I don't know how these two functions differ but I can recommend you to use `fromiter`
for small arrays.
