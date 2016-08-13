LIGHT SPACE PERSPECTIVE SHADOW MAPS
===================================

This programn is a demonstration of the Light Space Perspective Shadow Mapping
algorithm described in the respective paper. Additional information and sample 
images are available at 

http://www.cg.tuwien.ac.at/research/vr/lispsm/

Copyright and Disclaimer:

This code is copyright Vienna University of Technology, 2004.


Please feel FREE to COPY and USE the code to include it in your own work, 
provided you include this copyright notice.
This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. 

Authors: 

Daniel Scherzer (scherzer@cg.tuwien.ac.at)
Michael Wimmer (wimmer@cg.tuwien.ac.at)

Date: October 14, 2004

----------
Input
----------

Help for keybord and mouse is available with F1.
In the light view, the view frustum is shown as a semitransparent body and the
view vector as a red line emanating from the near plane.


----------
The scene
----------

The scene used in this program is designed to show all known shadow mapping problems:
projection aliasing, perspective aliasing and offset problems. 
For instance, self shadowing artefacts at the spiky cones are visible, and offset 
problems can be generated at certain viewing angles towards the hills. 

The LiSPSM algorithm is designed to reduce perspective aliasing only. Projection aliasing
and offset problems appear just like in uniform or perspective shadow maps.

To reduce offset problems we implemented two strategies: shadow map rendering with polygon
offset enabled and back side rendering. this can be switched with the <p> key.
Hint: Some driver implementations give better results with back side rendering, others with 
polygon offset.

Note that due to the perspective warp, a constant polygon offset may not be sufficient in some 
scenes. This can be fixed using a fragment shader that carries over the original z-coordinate 
and uses this coordinate (including a bias) for comparison, however this functionality is not
implemented in this demo.

----------
Installation
----------

A binary for Win32 is included.

The program should compile under Windows and Linux, a CMAKE makefile for multi-platform
compilation is included (see www.cmake.org for details).
For Linux, you need to have a working GLUT and GLEW installation.

----------
Structure
----------

This demo is written in C and uses OpenGL in order to keep it as universal as possible. 
It has few external needs like limits.h, stdlib.h, math.h, string.h, stdio.h and a 
C-compiler ;)

It expects the ARB_Shadow extension (GeForce3/Radeon 9500 or higher for hardware support) 
and uses ARB_Shadow_ambient if available.

The program and especially the mathematical routines are not designed for speed or
efficiency, but for simplicity and clarity.

The shadow map size is set to 512x512 pixel. As a rule of thumb the shadow map resolution 
should be twice the size of the viewport. In this demo we choose this small size for 
compatibility, because we don't use the PBuffer extension (because this would blow up the
code significantly) and have to render the shadow map into the back-buffer. 
The resulting shadows are therefore not as good as the results shown in our sample 
pictures, where we used a pBuffer with twice the size of the viewport.

LiSPSM.c:    contains the algorithm described in the paper

glInterface: includes gl, glut and glew headers
Main.c:      contains the OpenGl setup code, glut stuff, and basic shadow mapping code;
Main.h:      declares global variables for use in Main.c and LispSM.c
DataTypes.h: declares basic geometric datatypes
DataTypes.c: defines basic routines for these datatypes
MathStuff.h: declares mathematical routines used in the rest of the program
MathStuff.h: defines some handy mathematical routines
SceneData.h: declares some scene related datastructures and routines
SceneData.c: handles the drawing of the scene;

If you find any problem with the code or have any comments, we would like to hear from you!

----------
Change Log
----------

19. Aug. 2004: the variable name "cosGamma" was corrected to "sinGamma"
20. Sep. 2004: float -> double for better numerical stability
26. Sep. 2004: solved problem with certain special cases with a low sun setting.
               the solution is to use the "body vector" of the intersection body B as up vector
28. Sep. 2004: shadow matrix can be calculated with viewDir as up vector or the body vector of B as up vector
               (switch with 'n' key). code changes are marked with a //CHANGED comment
