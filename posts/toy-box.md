---
title: "Toy Box"
description: "Repositories and code snippets"
date: 1970-01-01T00:00:00+00:00
date-modified: 2025-07-25T00:00:00+00:00
categories:
  - repository
  - codesnippet
draft: false
---

## Repositories

In this section, repositories are summarized by programming languages.

### C++

#### Libraries

<https://github.com/Hasenpfote/dualcomplex/>

"dualcomplex" is a cross-platform C++ template library that provides operations for dual complex numbers. It can be used as a header-only library and supports C++11 or later.

<https://github.com/Hasenpfote/dualquat/>

"dualquat" is a cross-platform C++ template library that provides operations on dual quaternions using Eigen. It can be used as a header-only library and supports C++11 and later.

<https://github.com/Hasenpfote/plucker/>

"plucker" is a cross-platform C++ template library that provides operations on Plücker coordinates of lines. It can be used as a header-only library and supports C++11 and later.

#### Examples

<https://github.com/Hasenpfote/CMakeExamples/>

This is a collection of examples that use CMake for each case of a C++ project.

<https://github.com/Hasenpfote/CPPExamples/>

This is a collection of examples that use C++11.

- BitonicSort

- ComparingFloatingPointNumbers

- ConvertString

- ExpressionTemplate

- HalfPrecisionFloatingPoint

- LUDecomposition

- Logger

- PimplIdiom

- ParseFntFile

- Range

- ServiceLocator

- Singleton

- SpecificInterface

- ThreadPool

- Other

<https://github.com/Hasenpfote/cpp_examples/>

This is a collection of examples that use C++14 or C++17.

- circular_buffer

Circular buffer implemented using only the standard library.

- stack_resource

A stack memory allocator using only the standard library.

- pubsub_event

This is a Publish-Subscribe messaging model implemented using only the standard library.

- dual

Header-only class template for dual numbers using only the standard library.

<https://github.com/Hasenpfote/mark-and-sweep-gc/>

This is an example of a simple mark-and-sweep garbage collection using C++.

<https://github.com/Hasenpfote/OpenGLExamples/>

This is a collection of examples of real-time rendering techniques using OpenGL.

It supports C++11 or later and OpenGL 4.x.

- AdaptiveTerrainTessellation

- BillboardBeam

- Bloom

- Dithering

- KawaseBlur

- LightStreak

- RadialBlur

- ToneMapping

- Other

### Java

#### Examples

<https://github.com/Hasenpfote/RecursiveDimensionalClustering/>

This is an example of Recursive Dimensional Clustering, one of the fast algorithms for finding collisions or clusters in a dataset.

### Python

#### Modules

<https://github.com/Hasenpfote/fpq/>

This package provides modules for manipulating floating point numbers quantization using NumPy.

<https://github.com/Hasenpfote/image_packer/>

Pack multiple images of different sizes or formats into one image.

<https://github.com/Hasenpfote/malloc_tracer/>

This is a debugging tool for tracing malloc that occurs inside a function or class.

<https://github.com/Hasenpfote/perfbench/>

perfbench measures execution time of code snippets with Timeit and uses Plotly to visualize the results.

#### Examples

<https://github.com/Hasenpfote/python-poetry-example/>

A simple example of how to use pyenv + poetry + tox + pytest.

<https://github.com/Hasenpfote/quaternion/>

This package provides a class for manipulating quaternion objects.

## Code snippets

This section summarizes code snippets by themes/topics.

### Mathematics

| Topic              | Links                                                                                                                                                           |                                                                                                                                                                  | Language / Libraries                         |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------- |
| Complex number     | [Representation and interpolation of rotations](https://gist.github.com/Hasenpfote/566a04b7a5f9563921dff7fe1c1b5a01)                                            |                                                                                                                                                                  | C++                                          |
| Geometry           | [Delaunay triangulation](https://gist.github.com/Hasenpfote/18c192001a49a78defa991e7c6c71354)                                                                   |                                                                                                                                                                  | Python                                       |
|                    | [Normal vectors to a 2D curve](https://gist.github.com/Hasenpfote/9930495e79b708f9d6d0752764e06f88)                                                             | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/Hasenpfote/9930495e79b708f9d6d0752764e06f88) | Python<br>- matplotlib<br>- numpy<br>- sympy |
|                    | [Normal vectors to a surface](https://gist.github.com/Hasenpfote/11a40a555e82599d1e2d231c84d06fd7)                                                              | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/Hasenpfote/11a40a555e82599d1e2d231c84d06fd7) | Python<br>- matplotlib<br>- numpy<br>- sympy |
| Analysis           | [Taylor series](https://gist.github.com/Hasenpfote/42696bfece3d9f05f002dc41d8e8c38d)                                                                            | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/Hasenpfote/42696bfece3d9f05f002dc41d8e8c38d) | Python<br>- matplotlib<br>- numpy<br>- sympy |
| Vector calculus    | [Laplacian](https://gist.github.com/Hasenpfote/a97842b21700f44d82052548bf015ea0) ([utils](https://gist.github.com/Hasenpfote/e47a82d73dcf6ce84920b1e8106c477d)) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/Hasenpfote/a97842b21700f44d82052548bf015ea0) | Python<br>- matplotlib<br>- numpy            |
|                    | [Frenet–Serret formulas in 2D](https://gist.github.com/Hasenpfote/3e6b5605e56c4788d7ef46d2173ca20a)                                                             | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/Hasenpfote/3e6b5605e56c4788d7ef46d2173ca20a) | Python<br>- matplotlib<br>- numpy<br>- sympy |
|                    | [Frenet–Serret formulas](https://gist.github.com/Hasenpfote/1d8d0f745fd03e32e9f75bea1782d8cc)                                                                   | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/Hasenpfote/1d8d0f745fd03e32e9f75bea1782d8cc) | Python<br>- matplotlib<br>- numpy<br>- sympy |
| Set theory         | [Pairing function](https://gist.github.com/Hasenpfote/f9e71ceecf7a185422fe22000ce3cd2f)                                                                         | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/Hasenpfote/f9e71ceecf7a185422fe22000ce3cd2f) | Python<br>- matplotlib<br>- numpy            |
| Probability theory | [Inverse transform sampling](https://gist.github.com/Hasenpfote/09701f65d0c841d9ead91328e84ff2ff)                                                               | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/Hasenpfote/09701f65d0c841d9ead91328e84ff2ff) | Python<br>- matplotlib<br>- numpy            |
|                    | [Monte Carlo Integration](https://gist.github.com/Hasenpfote/90b6e3ba1e8249e7f0d51198c8e6bf8d)                                                                  | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/Hasenpfote/90b6e3ba1e8249e7f0d51198c8e6bf8d) | Python<br>- matplotlib<br>- numpy<br>- scipy |

### Physics

| Topic               | Links                                                                                                                                                           |                                                                                                                                                                                                   | Language / Libraries |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------- |
| Rigid body dynamics | [Translational and rotational motions](https://gist.github.com/Hasenpfote/034cc499827da562fed3c8f676b868f0#file-rigid-body-dynamics-ipynb)                      | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/Hasenpfote/034cc499827da562fed3c8f676b868f0#file-rigid-body-dynamics-ipynb)   | Python<br>- numpy    |
|                     | [Translational and rotational motions (using quaternion)](https://gist.github.com/Hasenpfote/034cc499827da562fed3c8f676b868f0#file-rigid-body-dynamics-2-ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/Hasenpfote/034cc499827da562fed3c8f676b868f0#file-rigid-body-dynamics-2-ipynb) | Python<br>- numpy    |

### Computer science

| Topic                 | Links                                                                                                                                                                                                                                                                                         |                                                                                                                                                                  | Language / Libraries                                     |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| Floating point number | [Floating-point comparison](https://gist.github.com/Hasenpfote/a7b836cbc5eaebf15dff65a8045a1701)                                                                                                                                                                                              |                                                                                                                                                                  | C++                                                      |
| Color                 | [CIE1931_color_space](https://gist.github.com/Hasenpfote/fb262185313b0204893483add22d45a5)<br>[CIE_daylight_components](https://gist.github.com/Hasenpfote/78264ec0046a0d8e0d85ad683c35b8c4), [BabelColor_spectral_data](https://gist.github.com/Hasenpfote/36650bfca3aa6815382f3e96567ec43a) |                                                                                                                                                                  | Python<br>- matplotlib<br>- numpy<br>- pandas<br>- scipy |
| Noise                 | [Perlin noise - 1D/2D/3D/4D](https://gist.github.com/Hasenpfote/b01238342a8625e2961b040d55d96b8d)                                                                                                                                                                                             | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/Hasenpfote/b01238342a8625e2961b040d55d96b8d) | Python<br>- matplotlib<br>- numpy                        |
| Sort                  | [Bitonic sort](https://gist.github.com/Hasenpfote/9e6935560778784ba80c0d497d7655c0)                                                                                                                                                                                                           |                                                                                                                                                                  | C++                                                      |
| Russian Roulette      | [Thoughts on Russian Roulette](https://gist.github.com/Hasenpfote/3703d424966523a2e3aba4e1007529d9)                                                                                                                                                                                           | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/Hasenpfote/3703d424966523a2e3aba4e1007529d9) | Python<br>- altair<br>- numpy<br>- pandas                |
| k-d tree              | [Thoughts on k-d tree](https://gist.github.com/Hasenpfote/7733dae3b31ea1d2a7b9f7145105ac84)                                                                                                                                                                                                   | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/Hasenpfote/7733dae3b31ea1d2a7b9f7145105ac84) | Python<br>- matplotlib<br>- numpy<br>- anytree           |

### Python

| Topic      | Links                                                                                                                  |                                                                                                                                                                   |
| ---------- | ---------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Python     | [How to limit concurrency with asyncio](https://gist.github.com/Hasenpfote/1028784b12b8bb2f48349b5d794559fe)           | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/Hasenpfote/1028784b12b8bb2f48349b5d794559fe)  |
|            | [How to hook properties with Python3 dataclasses](https://gist.github.com/Hasenpfote/903c3fd2ee7db16f31e18860458a6d4b) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/Hasenpfote/903c3fd2ee7db16f31e18860458a6d4b)  |
| matplotlib | [Customized colormap](https://gist.github.com/Hasenpfote/071e05c854e2b7d04cc3a9642778374f)                             | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/Hasenpfote/071e05c854e2b7d04cc3a9642778374f)  |
|            | [Discrete colormap](https://gist.github.com/Hasenpfote/0e6ea4e82564e3b284b08bde05b73c10)                               | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/Hasenpfote/0e6ea4e82564e3b284b08bde05b73c10)  |
| pytorch    | [Differential](https://gist.github.com/Hasenpfote/95dddcf6cdf1365e8b227ff46e3140de)                                    | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/Hasenpfote/95dddcf6cdf1365e8b227ff46e3140de)  |
|            | [Confusion matrix](https://gist.github.com/Hasenpfote/dced523e87dba58363e36f4443f8ed02)                                |  [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/Hasenpfote/dced523e87dba58363e36f4443f8ed02) |
