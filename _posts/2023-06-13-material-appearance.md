---
title: 'Material Appearance Representation'
date: 2023-06-13
teaser: images/BRDF.png
description: Material properties define its appearance. How do we represent the appearance of materials?
---

The realistic rendering of scenes requires a precise description of how real-world materials scatter light. Here, the appearance of a material can be defined as a function of how that material interacts with light. The material can reflect light or cause it to diffuse beneath the surface, a complex phenomena known as subsurface scattering.

Reflectance describes the amount of light reflected from the surface of a material. In general, the amount of reflected light may change at each point on the surface, and for each possible direction of incoming (w_i) and outgoing (w_o) light. Therefore, a complete way of representing the reflectance properties of a material is by defining a 6-dimensional function f(w_i, w_o, x) of how light is reflected at location x of the surface, which is known as spatially-varying bidirectional reflectance distribution function (SVBRDF).

<p align="center">
  <img src="/images/BRDF.png" width="75%" /><br/>
  <br/>Fig. 1: How much incoming light is reflected from the surface is defined by the bidirectional reflectance distribution function (BRDF).[1]<br/>
</p>


## References
[1] Physically Based Rendering: From Theory To Implementation, © 2004-2021 Matt Pharr, Wenzel Jakob, and Greg Humphreys
