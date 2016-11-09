---
layout:     post
title:      "Python : customize setup actions"
author:     realitix
summary:    How to extend setup actions
category:   python
thumbnail:  exchange
---

## Introduction

Sometimes you want to do strange things not planned by software designer,
and happily, Python is the best language for introspection. In this post,
we are going to extend functionalities of the `setup.py` file that you may
have created to deploy your Python application.


## Time to fly!

Let's create a simple `setup.py` to start:

```python
from setuptools import setup, find_packages

setup(
    name="test",
    version="1",
    packages=find_packages(),
    author="realitix",
    author_email="mail",
    description="test",
    cmdclass={}
)
```

Like you can see, I have added a `cmdclass` property to my `setup` function.
When you type a setup command like `python setup.py clean`, the `distutils`
module (used by `setuptools`) will search for the corresponding key in `cmdclass`
property, which is `cmdclass['clean']` in our case. The value corresponding to
this key must be a class callable by `distutils`.

If this key doesn't exist in the `cmdclass` dictionnary, like `install` for example,
`distutils` will search through its embedded commands to find the good one.

If you follow me from the start, you should be saying:

> OK, I just have to find the command and extend it.

You are right!


## Through the path of hidden commands...

Ok I tell you the secret, to find theses commands, just find `distutils` path:

```bash
python -c "import distutils; import os; print(os.path.dirname(distutils.__file__))"
```

Then go in `<distutils_path>/commands` and there you are.


## Let's get back to the point

Now that you understand how it works, we can easily extend the `setup`
commands. Let's extend the `clean` command.

```python
from distutils.command.clean import clean
from setuptools import setup, find_packages


class MyClean(clean):
    def run(self):
        # Execute the classic clean command
        super().run()

        # Just add a print for the example
        print("exemple")

setup(
    name="test",
    version="1",
    packages=find_packages(),
    author="realitix",
    author_email="mail",
    description="test",
    cmdclass={'clean': MyClean}
)
```

And... it works like a charm!


## To go further

Do you enjoy the integrated `distutils` command ? Yes for sure !
But what if you could create your custom command ? It's not a surprise
but yes you can do it and you know how to do it !
