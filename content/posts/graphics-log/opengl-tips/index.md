---
title: Practical tips for efficient development with OpenGL
seo_title: Practical tips for efficient development with OpenGL
summary: 
description: 
slug: opengl-tips
author: Vihanga Marasinghe

draft: false
date: 2024-01-03T12:39:33+05:30
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

### Introduction

As you navigate the intricacies of OpenGL development, adopting smart practices can significantly enhance your efficiency and code quality.  To enhance your proficiency in the aspect of graphics programming, let's explore some valuable tips derived from practical experiences in my CS308 lab sessions. I hope these tips would help you set up a good development workflow.


### 1. OpenGL Code Template for Project Initialization:
   Having a well-structured code template for OpenGL projects is essential. The provided template is one example which includes components such as initialization, display function, keyboard input handling, timer function, and window reshaping.You can use any template suited to your needs.This not only saves time by providing a consistent starting point but also helps enforce good programming practices.

``` cpp
    
    #include <GL/glut.h>  
    #include <math.h>

    void init() {
        glClearColor(0.0f, 0.0f, 0.0f, 1.0f);

    }

    void drawAxes() {

        
    }
    void DrawGrid() {
        
    }

    void drawObject() {

    }

    void display() {

        glClear(GL_COLOR_BUFFER_BIT);
        glutSwapBuffers();

    }

    void keyboardSpecial(int key, int x, int y) {
        
        glutPostRedisplay();
    }

    void keyboard(unsigned char key, int x, int y) {

        glutPostRedisplay();

    }

    void Timer(int x) {
        
        glutPostRedisplay();

        glutTimerFunc(60, Timer, 1);
    }

    void reshape(GLsizei w, GLsizei h) {

    }

    int main(int argc, char** argv) {

        glutInit(&argc, argv);
        glutInitDisplayMode(GLUT_DOUBLE);
        glutInitWindowSize(500, 500);
        glutInitWindowPosition(150, 150);
        glutCreateWindow("A 3D Graphic");
        glutDisplayFunc(display);
        glutReshapeFunc(reshape);

        glutKeyboardFunc(keyboard);
        glutSpecialFunc(keyboardSpecial);

        glutTimerFunc(60.0, Timer, 1);
        init();
        glutMainLoop();

        return 0;
    }
```

   ### 2. Drawing Grids and Axes for Object Placement:

   Visual aids like grids and axes can significantly assist in object placement, translation, and rotation. Implementing functions like `drawAxes()` and `DrawGrid()` allows for a better understanding of spatial relationships within the scene, aiding in the precise positioning of objects.

   ```cpp
   void drawAxes() {

    glBegin(GL_LINES);

    glLineWidth(1.5);

    glColor3f(1.0, 0.0, 0.0); // RED - X
    glVertex3f(-40, 0, 0);
    glVertex3f(40, 0, 0);

    glColor3f(0.0, 1.0, 0.0); //GREEN -Y
    glVertex3f(0, -40, 0);
    glVertex3f(0, 40, 0);

    glColor3f(0.0, 0.0, 1.0); //BLUE -Z
    glVertex3f(0, 0, -40);
    glVertex3f(0, 0, 40);

    glEnd();
    
   }

   void DrawGrid() {
    GLfloat ext = 40.0f;
    GLfloat step = 1.0f;
    GLfloat yGrid = -0.4f;
    GLint line;

    glBegin(GL_LINES);
    for (line = -ext; line <= ext; line += step)
    {
        glVertex3f(line, yGrid, ext);
        glVertex3f(line, yGrid, -ext);

        glVertex3f(ext, yGrid, line);
        glVertex3f(-ext, yGrid, line);
    }
    glEnd();
   }
   ```

{{< figure src="axes.PNG" caption="axes">}}

{{< figure src="grid.PNG" caption="grid">}}


### 3. Using Reshape Function for Window Size Changes:

   To prevent warping and maintain a proper aspect ratio when resizing the window, leverage the `reshape` function. This ensures that the perspective projection frustum is adjusted accordingly, providing a consistent and visually pleasing experience.

   ```cpp
     reshape(GLsizei w, GLsizei h)
    {
        glViewport(0, 0, w, h);
        GLfloat aspect_ratio = h == 0 ? w / 1 : (GLfloat)w / (GLfloat)h;

        glMatrixMode(GL_PROJECTION);
        glLoadIdentity();

        gluPerspective(120.0, aspect_ratio, 1.0, 100.0);

    }
   ```

### 4. Organizing Code with Header Files:
   As projects grow in complexity, it becomes crucial to maintain a well-organized codebase. Using header files to separate different parts of the project helps avoid clutter and enhances code readability. This practice simplifies maintenance and collaboration among developers.

   ```cpp
    #include "pinetree.h"
    #include "cablecarpole.h"
    #include "sled.h"
   ```

### 5. Leveraging Vertex Arrays for Efficient Object Creation:

In OpenGL programming, optimizing the creation of objects is essential for both performance and maintainability. One powerful technique to achieve this is by utilizing vertex arrays to manage sets of vertices efficiently. The provided array, float vertices[][3], exemplifies a collection of vertices in 3D space.

``` cpp
    float vertices[][3] =
        { {-1,-1,1}, {-1,1,1} ,{1,1,1},{1,-1,1} , {1,-1,-1 } ,{1,1,-1},{-1,1,-1},{-1,-1,-1} ,
            {-1,-1,0},{0,-1,1},{1,-1,0},{0,-1,-1 },{1,0,-1},{0,1,-1} , {-1,0,-1},{-1,1,0},{-1,0,1},{1,1,0},{1,0,1},{0,1,1}
        };
 ```
Each vertex could then be refered later using array indices without redefining a new array each time it is needed. 

### 6. Efficient Surface Generation Methods:

Reducing code repetition is vital for efficient development. Define methods for generating surfaces with varying numbers of vertices. This helps streamline the creation of 3 and 4-faced surfaces, promoting code reusability and readability.

   ```cpp
    void surface4(int v1,int v2,int v3,int v4) {
        glPushMatrix();
        glBegin(GL_POLYGON);
        glVertex3fv(vertices[v1]);
        glVertex3fv(vertices[v2]);
        glVertex3fv(vertices[v3]);
        glVertex3fv(vertices[v4]);
        glEnd();
        glPopMatrix();
    }
   ```

### Conclusion:

Optimizing object modeling in OpenGL involves a combination of code structuring, visual aids, and efficient coding practices. By implementing the tips provided, you'll not only enhance the efficiency of your graphics programming but also contribute to a more maintainable and scalable codebase.