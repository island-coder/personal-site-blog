---
title: Animated Clock using 2D Graphics
seo_title: Animated Clock using 2D Graphics
summary: 
description: 
slug: cg-2
author: Vihanga Marasinghe

draft: false
date: 2023-11-01T14:22:42+05:30
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

# Creating an Animated Clock Using OpenGL in C++

## Introduction

Let's take a look at how we can code a simple animated clock in OpenGL.

## Initialization

In this clock program, we start by including the necessary libraries and setting up essential variables. 

```c++
#include <windows.h>
#include <GL/glut.h>
#include <math.h>

float PI = 3.14;

int animationFactor = 0;
int clockMinute=0;

double minuteHandRotation=0;
double hourHandRotation =0;


void init() {
    glClearColor(0.2, 0.95, 0.6, 1.0);
    glClear(GL_COLOR_BUFFER_BIT); //clearing color buffer
}
```

## Drawing Functions

Creating a visual representation of the clock involves several drawing functions. Let's briefly cover the main components:

- **Drawing the Clock Disk**: This function is responsible for rendering the circular face of the clock.

```c++
void drawDisk(double radius, int n) {
    double angle = 0;
    glPushMatrix();
    glColor3f(1, 1, 1);
    glBegin(GL_POLYGON);
    for (int c = 0; c <= n; c++) {
        double x = radius * cos(angle);
        double y = radius * sin(angle);
        glVertex2d(x, y);
        angle = angle + ((2 * PI) / n);
    }
    glEnd();
    glPopMatrix();
}
```

- **Drawing Clock Marks**: To mark the hours, we draw lines that signify each hour on the clock face.

```c++
void drawMarks(double radius){
    glPushMatrix();
    glColor3f(0, 0, 0);
    glBegin(GL_LINES);
    double angle=0;
    for (int c = 0; c <= 12; c++) {
        double x = radius * cos(angle);
        double y = radius * sin(angle);

        glVertex2d(0, 0);
        glVertex2d(x, y);
        angle = angle + ((2 * PI) / 12);
    }
    glEnd();
    glPopMatrix();
}
```

- **Drawing the Hour Hand**: The hour hand is drawn and rotated according to the current time, moving every five minutes.

```c++
void drawHourHand(double radius){

    if(clockMinute==5){
        hourHandRotation+= (360 /  12);
        clockMinute=0;
    }

    glPushMatrix();
    glColor3f(1, 0, 0);
    glRotated(-1*hourHandRotation,0,0,1);
    glBegin(GL_POLYGON);
    glVertex2d(0, radius);
    glVertex2d(radius*0.1, 0);
    glVertex2d(-radius*0.1, 0);

    glEnd();
    glPopMatrix();

}
```
- **Drawing the Minute Hand**: Similarly, the minute hand is drawn and rotated, advancing every minute.

```c++

void drawMinuteHand(double radius){
    glPushMatrix();
    glColor3f(0.8, 0.5, 0);
    glRotated(-1*minuteHandRotation,0,0,1);
    glBegin(GL_POLYGON);
    glVertex2d(0, radius*0.5);
    glVertex2d(radius*0.05, 0);
    glVertex2d(-radius*0.05, 0);
    glEnd();
    glPopMatrix();

    minuteHandRotation += (360 /  60);
}
```

- **Drawing the Complete Clock**: The final clock is assembled by combining the disk, marks, hour hand, and minute hand.

```c++
void drawClock(int radius){
    drawDisk(radius,32);
    drawMarks(radius);
    drawHourHand(radius);
    drawMinuteHand(radius);
}
```
## Reshaping and Display Functions

These important utility functions handle the window resizing and actual rendering of the clock. The reshaping function ensures that the clock maintains its aspect ratio, while the display function takes care of rendering the clock and axes.

```c++
void reshape(GLsizei w, GLsizei h) {
    glViewport(0, 0, w, h);
    //calculate aspect ratio
    if (h == 0)
        h = 1;
    GLfloat aspect = (GLfloat)w / (GLfloat)h;

    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    if (w >= h)
        gluOrtho2D(-10 * aspect, 10 * aspect, -10, 10);
    else
        gluOrtho2D(-10, 10, -10 / aspect, 10 / aspect);
}

void display() {
    glClear(GL_COLOR_BUFFER_BIT); // to avoid tracing 
    glLineWidth(2.0); //Setting the line width

    //y-axis
    glColor3f(1.0, 0.0, 0.0);
    glBegin(GL_LINES);
    glVertex2f(0.0, 100.0);
    glVertex2f(0.0, -100.0);
    glEnd();

    //x-axis
    glColor3f(1.0, 0.0, 0.0);
    glBegin(GL_LINES);
    glVertex2f(100.0, 0.0);
    glVertex2f(-100.0, 0.0);
    glEnd();

    drawClock();

    glutSwapBuffers(); //swapping the buffers

}
```

## Animation

The animation function is a crucial part of this project. It updates the clock's minute and hour hands in real-time, creating the illusion of a working clock.

## Conclusion

Creating an animated clock using OpenGL in C++ is not only a fun project but also a great way to explore the principles of computer graphics and animation.
