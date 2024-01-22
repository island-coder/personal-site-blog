---
title: Basic Texturing
seo_title: Basic Texturing
summary: 
description: 
slug: basic-textures
author: Vihanga Marasinghe

draft: false
date: 2023-11-10T11:17:36+05:30
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

## Introduction

In computer graphics, textures play a pivotal role in bringing life and realism to 3D scenes. Texturing is simply the process of applying images or patterns to the surfaces of 3D models.

## Why Textures Matter

Textures provide a way to mimic real-world surfaces, adding depth, complexity, and richness to our digital creations. Whether it's the grain of wood, the roughness of a stone wall, or the intricate details of fabric, textures enable us to recreate these visual elements in a virtual space. By applying textures, we can transform a simple sphere into a realistic planet, a plain cube into a weathered brick wall, or a flat surface into a lush field of grass.

## Texture Coordinates: Mapping 2D Images onto 3D Surfaces

Before we dive into OpenGL and SOIL2, let's touch on texture coordinates. These are the coordinates used to map a 2D image onto a 3D surface. Each vertex in a 3D model has associated texture coordinates, determining how the texture is wrapped around the surface. Texture coordinates range from (0,0) to (1,1), with (0,0) typically representing the bottom-left corner of the texture and (1,1) representing the top-right corner.

## Implementation with OpenGL and SOIL2: Bringing Textures to Life

Now, let's look into the practical side of texturing using OpenGL and the SOIL2 library. Below is a straightforward code snippet that demonstrates how to apply a texture to a sphere. The SOIL2 library simplifies the process of loading textures.

### Loading the textures

``` c++
#include <SOIL.h>
#include <GL/glut.h>

GLuint texture_wooden, texture_metallic, texture_mars;

void loadTexture()
{
    // Load wooden texture
    texture_wooden = SOIL_load_OGL_texture(
        "wooden.jpg", SOIL_LOAD_AUTO, SOIL_CREATE_NEW_ID, SOIL_FLAG_MIPMAPS | SOIL_FLAG_INVERT_Y
    );

    // Load metallic texture
    texture_metallic = SOIL_load_OGL_texture(
        "metallic.jpg", SOIL_LOAD_AUTO, SOIL_CREATE_NEW_ID, SOIL_FLAG_MIPMAPS | SOIL_FLAG_INVERT_Y
    );

    // Load mars texture
    texture_mars = SOIL_load_OGL_texture(
        "mars.jpg", SOIL_LOAD_AUTO, SOIL_CREATE_NEW_ID, SOIL_FLAG_MIPMAPS | SOIL_FLAG_INVERT_Y
    );

    // Error handling
    if (!texture_wooden || !texture_metallic || !texture_mars)
    {
        printf("Texture loading failed: %s\n", SOIL_last_result());
    }
}

```
### Rendering Textured Objects

``` c++
void drawSphereWithTexture(float radius, GLuint texture)
{
    GLUquadric* quad = gluNewQuadric();

    gluQuadricTexture(quad, GL_TRUE); //gluQuadricTexture handles texture cordinate mapping in this case ,no need of specifying them manually

    glBindTexture(GL_TEXTURE_2D, texture); // binds the texture to the object 

    gluSphere(quad, radius, 32, 32);

    gluDeleteQuadric(quad);
}

void drawScene()
{
 
    glPushMatrix();
    drawSphereWithTexture(2, texture_metallic);
    glPopMatrix();

    glPushMatrix();
    glTranslatef(3, 0, -3);
    drawSphereWithTexture(2, texture_wooden);
    glPopMatrix();

    glPushMatrix();
    glTranslatef(3, 0, 3);
    drawSphereWithTexture(2, texture_mars);
    glPopMatrix();
}

```

### Results

{{< figure src="spheres1.PNG" >}}

{{< figure src="spheres2.PNG" >}}

{{< figure src="spheres3.PNG">}}
