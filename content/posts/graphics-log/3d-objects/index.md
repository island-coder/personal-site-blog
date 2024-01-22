---
title: Techniques for Object Creation 
seo_title: Techniques for Object Creation
summary: 
description: 
slug: 3d-objects
author: Vihanga Marasinghe

draft: false
date: 2024-01-01T13:56:46+05:30
lastmod: 
expiryDate: 
publishDate: 

feature_image: 
feature_image_alt: 

categories:
tags:
    - Computer Graphics
    - OpenGL
series: 
    - Graphics Log


toc: true
related: true
social_share: true
newsletter: false
disable_comments: false
---

### Introduction:

In this article I'll be presenting some techniques that we have used in our lab sessions to construct 3D objects with OpengGL.OpenGL provides the ability to create some common predefined shapes using built in methods,but since we often need to model custom objects we have to find some ways to make this process easier.

### Creating Complex Shapes with Basic 3D Primitives:

When creating some complex objects its easier to visualize them as a composition of certain basic shapes such as spheres,cubes,cuboids,cones and cylinders.This techniques could be used in OpenGL to build up complex objects using these common shapes for which there are built in methods present in the library.

{{< figure src="skilift.PNG" caption="Ski Lift object created using a combination of basisc shapes">}}


### Building Objects through Vertex Specification:

Another technique of 3D object creation involves the explicit definition of vertices. When following this approach, I found it helpful to sketch and plan the arrangement of vertices.This would make the implementation coding easier.


{{< figure src="cube.PNG" caption="Cube created by specifying vertex points">}}


Additionally, in some sitations it's useful to have a known solid object, like a cube, as a reference to shape more intricate forms.An example for using this technique is creating a shape such as a 'atapattama' or Vesak lantern using points on a cube as reference points.

{{< figure src="cube-ref.PNG">}}

{{< figure src="atapattama.PNG" caption="Shape created using points on a cube as reference points.">}}


### Conclusion 

As you embark on the adventure of creating 3D objects in OpenGL, the versatility of approaches at your disposal becomes clear. Whether you opt for the artistic fusion of basic shapes or thee specification of vertices, each method offers a unique avenue for bringing your digital creations to life. 

