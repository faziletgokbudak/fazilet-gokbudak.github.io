---
title: 'Material Appearance Representation'
date: 2023-06-13
teaser: images/BRDF.png
description: Material properties define its appearance. How do we represent the appearance of materials?
---

The realistic rendering of scenes requires a precise description of how real-world materials scatter light. Here, the appearance of a material can be defined as a function of how that material interacts with light. The material can reflect light or cause it to diffuse beneath the surface, a complex phenomena known as subsurface scattering.

## Reflectance Mechanisms
The appearance of materials is determined by its reflectance properties. To represent and model the reflectance, first we need to understand how light interacts with the surface:

### Surface reflection (Specular)
Surface reflection occurs when the light is reflected at the surface. So, the interface is the same point as where the reflectance occurs. It is also known as specular reflection. 

This type of reflection gives objects a glossy or shiny appearance, and materials of smooth surfaces, such as glass, mirrors or polished metal, usually cause specular reflection.

### Body reflection (Diffuse)
The second reflection mechanism is where some of the light enters the material and goes underneath the surface. As materials are typically non-homogeneous, there are particles of different types underneath the surface with different refractive indices. Hence, the light that enters the material gets reflected and refrected many times, bouncing around inside the material. This happens at the skin of the surface in such a random process that, eventually, causes to the light leaving the surface scattered in many different directions.

Body or diffuse reflection gives objects a matte appearance, and materials with non-homogenious medium, such as clay or paper, usually cause such reflection.

<p align="center">
  <img src="/images/reflection-mechanisms.png" width="66%" /><br/>
  <br/>Fig. 3: Surface vs Diffuse reflection [3].<br/>
</p>

In general, materials exhibit both reflection types. Hence, the reflection becomes hybrid, which is the combination of specular and diffuse reflection:

<p align="center">
  <img src="/images/hybrid-reflection.jpg" /><br/>
  <br/>Fig. 4: Hybrid reflection is a combination of both diffuse and specular reflection.<br/>
</p>

## Bidirectional reflectance distribution function (BRDF)

Reflectance describes the amount of light reflected from the surface of a material. In general, the amount of reflected light may change at each point on the surface, and for each possible direction of incoming ($\omega_i$) and outgoing ($\omega_o$) light. Therefore, a complete way of representing the reflectance properties of a material is by defining a 6-dimensional function f($\omega_i$, $\omega_o$, **x**) of how light is reflected at location **x** of the surface, which is known as spatially-varying bidirectional reflectance distribution function (SVBRDF).

<p align="center">
  <img src="/images/BRDF.png" width="75%" /><br/>
  <br/>Fig. 1: How much incoming light is reflected from the surface is defined by the bidirectional reflectance distribution function (BRDF) [1].<br/>
</p>

<p align="left">
  <img style="float: right;" src="/images/Four-angular-parameters-of-BRDF_W640.jpg" width="25%" /><br/>
</p>

Simplifying the SVBRDF, let's start with a 4-dimensional function of incident and reflected lights $\omega_i$ and $\omega_r$. The BRDF outputs the ratio of reflected radiance leaving the surface along $\omega_r$ to the irradiance coming from direction $\omega_i$. The directions are parameterized by two angles, azimuth angle $\phi$ and zenith angle $\theta$, making the function with four variables. So,


$$BRDF: f(\theta_i, \phi_i, \theta_r, \phi_r) = \frac{L(\theta_r, \phi_r)}{E(\theta_i, \phi_i)}$$ where $E(\theta_i, \phi_i)$ is the irradiance due to the source in direction $\omega_i$ and $L(\theta_r, \phi_r)$ is the radiance of surface in direction $\omega_r$. 

BRDF completely describes the reflectance properties of a point on the material surface. Some of its properties are:

- $f(\theta_i, \phi_i, \theta_r, \phi_r) > 0$. Radiance and irradiance cannot be negative. Hence, BRDF should be non-negative.
- Helmholtz reciprocity: $f(\theta_i, \phi_i, \theta_r, \phi_r) = f(\theta_r, \phi_r, \theta_i, \phi_i)$, which means BRDF doesn't change when the illumination and reflectance directions are flipped. That is, the reflectance properties of the surface at a point will be the same.

For **rotationally symmetric reflectance (isotropic surfaces)**, BRDF is a 3-D function: $f(\theta_i, \theta_r, \phi_i - \phi_r)$. If a surface is isotropic, it means that the brightness of the point of interest on the surface remains the same when the surface spins around its surface normal. In other words, the image/appearance of the material surface will not change when the surface is rotated. It happens especially when the surface has grooves which are directional. 

<p align="center">
  <img src="/images/isotropicvsanisotropic.jpeg" width="66%" /><br/>
  <br/>Fig. 2: The appearance of the isotropic sphere does not chance as it is rotated around its center [2].<br/>
</p>

## BRDF Models
Some commonly used reflectance models are as follows:

### Lambertian Model
The Lambertian model states that the brightness of a surface remains the same for all viewing directions. It represents the body (diffuse) reflection.

The surface BRDF will be the same no matter what direction you look at the surface from. So, the BRDF will be a constant:

$$f(\theta_r, \phi_r, \theta_i, \phi_i) = \frac{\rho_d}{\pi}$$, where $\rho_d$ is called albedo and it has a range of [0,1], 0 for a completely black surface and 1 for a completely white surface.
    
Lambertian model is one of the commonly used BRDF models in computer vision and graphics since it roughly covers a variety of real-world material surfaces. On a Lambertian surface, we get equal radiance in all directions. That is, the radiance $L$ is independent of viewing direction. However, as the angle of the incidence $\theta_i$ increases, the radiance value drops. This is due to the effect of irradiance. Therefore, the brigthest point on a Lambertian surface will be the one that the illumination comes up above (angle of incidence is 0).

### Ideal Specular Model
This model represents ideal specular reflection where the surface acts like a perfect mirror. Hence, all reflections happen at the interface, not underneath the surface. Also, all the energy of reflected light goes in a single direction. So, in such models, the viewer receives the light and observe the surface only if the angle of viewing direction is the same as the angle of incoming light about the surface normal.

### Phong Reflection Model

The Phong reflection model, one of the most popular reflection models in computer graphics, describes the reflection as a combination of the diffuse reflection of matte surfaces with the specular reflection of glossy surfaces. It is an emprical model based on Phong's observation that the highlights are rather small and intense for specular surfaces, whereas matte surfaces have large highlights that go off gradually.
Phong also takes into account the small amount of light bouncing in the scene and includes an ambient term:

<p align="center">
  <img src="/images/Phong_components_version_4.png" /><br/>
  <br/>Fig. 5: Visual illustration of Phong model. Here, the source light is white and ambient and diffuse components are blue. The specular color is white, illustrating the reflected light in small and intense highlights. The ambient term is uniform, independent of direction.<br/>
</p>


## References
[1] Physically Based Rendering: From Theory To Implementation, © 2004-2021 Matt Pharr, Wenzel Jakob, and Greg Humphreys

[2] Ren Ng, CS184/284A Lecture Notes, Lecture 12, Slide 22, https://cs184.eecs.berkeley.edu/sp16/lecture/reflection/slide_022

[3] https://astrobites.org/2020/10/22/specular-reflections-titan/

[4] https://3dtotal.com/tutorials/t/brief-consideration-about-materials-pedro-toledo-texturing-lighting

[5] https://en.wikipedia.org/wiki/Phong_reflection_model
