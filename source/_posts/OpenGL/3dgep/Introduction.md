https://www.3dgep.com/introduction-to-opengl-and-glsl/

# Introduction to OpenGL and GLSL

Posted on [February 13, 2014](https://www.3dgep.com/introduction-to-opengl-and-glsl/) by [Jeremiah](https://www.3dgep.com/author/jeremiah/)

![OpenGL](https://www.3dgep.com/wp-content/uploads/2012/05/OpenGL-logo-150x150.jpg)

OpenGL

In this article I will introduce the reader to the OpenGL rendering API (application programming interface). I will also introduce GLSL (OpenGL Shading Language). We will create a simple vertex shader and fragment shader that can be used to render very basic 3D primitives. By the end of this article you will know how to create a simple OpenGL application and render 3D objects using shaders.



Contents [[show](https://www.3dgep.com/introduction-to-opengl-and-glsl/#)]

# Introduction

OpenGL is a rendering API (application programming interface) that provides hardware accelerated (GPU) rendering functions. Unlike other popular graphics API’s (like DirectX), OpenGL is platform agnostic meaning that you can write an OpenGL application on one platform and the same OpenGL program can be compiled and run on another platform.

OpenGL was originally created by a company called Silicon Graphics in 1991 and released in 1992 [[2\]](https://www.3dgep.com/introduction-to-opengl-and-glsl/#OpenGL%20-%20Wikipedia). OpenGL was released as an open standard and a committee called the OpenGL Architecture Review Board (ARB) was setup to develop and maintain the OpenGL specification. In July 2006, the OpenGL Architecture Review Board transferred control of the OpenGL specification to the [Khronos](https://www.khronos.org/) group where it currently resides [[2\]](https://www.3dgep.com/introduction-to-opengl-and-glsl/#OpenGL%20-%20Wikipedia). Despite the transfer from the OpenGL Architecture Review Board, the ARB acronym is still used today to prefix the name of OpenGL core extensions.

# OpenGL 1.x Fixed-Function Pipeline

When OpenGL was first introduced, it featured a **fixed-function** rendering pipeline. The term “fixed-function” refers to the fact that all of the functions performed by OpenGL were “fixed” and could not be modified except through the manipulation of various rendering **states**. Using a fixed-function pipeline, the graphics programmer did not have full control over the various aspects of the rendering pipeline.

The following diagram illustrates the various stages of the fixed-function pipeline.

![OpenGL Fixed-Function Pipeline](https://www.3dgep.com/wp-content/uploads/2014/02/OpenGL-Fixed-Function-Pipeline1.png)

OpenGL Fixed-Function Pipeline

In this diagram, the blue nodes represent the stages of the rendering pipeline that are still used in the current OpenGL rendering pipeline. The green nodes represent stages of the fixed-function pipeline that have been replaced by different stages in the programmable shader pipeline in later versions of the OpenGL API.

## Vertex Data

The first step in the graphics application is sending the vertex data to the GPU for rendering. The vertex data represents the 2D or 3D objects that are to be rendered. In addition to the position of the vertex in 2D or 3D space several other attributes that describe the vertex can also be sent to the GPU. Using the fixed-function pipeline these attributes included vertex color, vertex normals, texture coordinates, and fog coordinate. The vertex data processed by the GPU is often referred to as the **vertex stream**.

## Primitive Processing

In the **Primitive Processing** stage, the vertex stream is processed per primitive. OpenGL has support for several types of primitives. These include points, lines, triangles, quads, and polygons (but quads and polygons are deprecated as of OpenGL 3.1). In addition to the classic primitive types, the patch primitive is introduced in OpenGL 4.0 to provide support for tessellation shaders.

![OpenGL Primitive Types](https://www.3dgep.com/wp-content/uploads/2011/02/OpenGL-Primitives.png)

OpenGL 2.1 Primitive Types

### POINTS

You can specify that OpenGL should treat the vertex data as points using the **GL_POINTS** OpenGL primitive enumeration. Points are drawn as either squares or circles of various sizes. Points can be used to render particle effects but it is more common to render particle sprites as quads instead of points.

### LINES

OpenGL provides several line primitive types [[3\]](https://www.3dgep.com/introduction-to-opengl-and-glsl/#OpenGL%202.1%20Reference):

- **GL_LINES**: Treat each pair of vertices as separate line segments. You must specify at least 2 vertices for a line to be drawn.
- **GL_LINE_STRIP**: Will draw an open connected series of lines where each vertex is connected by a line segment. The last vertex specified will not be automatically connected to the first vertex.
- **GL_LINE_LOOP**: Will draw a closed connected series of lines where each vertex is connected by a line segment. The last vertex will be automatically connected to the first vertex in the list.

### TRIANGLES

OpenGL provides several triangle primitive types [[3\]](https://www.3dgep.com/introduction-to-opengl-and-glsl/#OpenGL%202.1%20Reference):

- **GL_TRIANGLES**: Treat each triple set of vertices as a single triangle. Individual triangle primitives will not be connected.
- **GL_TRIANGLE_STRIP**: Will draw a connected group of triangles. For each additional vertex after the 3rd, the triangle is closed by adding an edge from the nthnth to the (n−2)th(n−2)th vertex.
- **GL_TRIANGLE_FAN**: This primitive type is useful for drawing circles with any number of vertices where the first vertex is the central vertex that is used to connect all additional vertices in the list.

### QUADS (DEPRECATED)

Prior to OpenGL 3.1, the quad primitive was provided to draw quadrilaterals. This primitive type was useful for drawing sprites (particle sprites or game sprites). The quad primitive type was depreciated in the OpenGL 3.1 specification and removed in the OpenGL 3.2 specification. The alternative is to specify a quad as a pair of connected triangles.

OpenGL 2.1 provided the following quad types [[3\]](https://www.3dgep.com/introduction-to-opengl-and-glsl/#OpenGL%202.1%20Reference):

- **GL_QUADS**: Used to draw a set of separated quadrilaterals (4-vertex polygons).
- **GL_QUAD_STRIP**: Used to draw a set of connected quadrilaterals.

### POLYGON (DEPRECATED)

Similar to the quads primitives types, the polygon primitives types have also been removed from the OpenGL specification as of OpenGL 3.2. Again, polygons should be defined as triangles instead.

To draw a connected (convex) polygon in OpenGL 2.1, you can use the **GL_POLYGON** primitive type.

## Transform and Lighting

The next stage of the OpenGL fixed-function pipeline is the **Transformation and Lighting** stage (often simply referred to as T&L). In this stage the vertex is transformed into view space by transforming the vertex position and vertex normal by the current model-view matrix (**GL_MODELVIEW**). After transforming the vertex position and vertex normal, lighting information is computed for each individual vertex. Since this stage happens for each vertex it is not capable of performing per-pixel lighting using the fixed-function pipeline. To achieve per-pixel lighting, you must use a fragment shader (using GLSL for example. This will be described later).

## Primitive Assembly

During the **Primitive Assembly** stage, OpenGL will convert the primitives from the basic primitive types into triangles. For example, if a triangle strip of consisting of **N** vertices, **N-2** triangles will be drawn [[3\]](https://www.3dgep.com/introduction-to-opengl-and-glsl/#OpenGL%202.1%20Reference). Primitives must also be transformed into viewport space and clipped to fit within the viewport boundaries. It is also possible that back-face culling will occur at this stage (if back-face culling is enabled).

## Rasterizer

The Rasterizer is responsible for converting the primitives from the primitive assembly into fragments (which eventually become the individual screen pixels). Vertex attributes are interpolated across the face of the vertex for attributes such as color, texture coordinates, and fog coordinates.

## Texture Environment

The texture environment will apply any bound textures to the fragments. OpenGL has support for several texture stages that can be applied to a single object. For an example of using multi-texture blending in the fixed-function pipeline, you can refer to my article titled [[Multi-textured Terrain in OpenGL\]](https://www.3dgep.com/?p=1116).

## Color Sum

The **Color Sum** stage is used to add-in a secondary color to the geometry after the textures have been applied. If the specular highlights are computed in the lighting stage (before textures are applied) the resulting effect simply makes the texture brighter instead of adding-in the specular highlights. This stage allows for the application of a secondary color in the final image [[4\]](https://www.3dgep.com/introduction-to-opengl-and-glsl/#Merging%20Textures%20with%20Specular%20Highlights).

## Fog

The fog stage was used to simulate the effect of geometry fading out as-if dimmed by fog. This is useful when rendering outdoor scenes when far away geometry will be clipped away by the far clipping plane (clipping planes are defined by the projection matrix). Instead of seeing the hard-edge of the far clipping plane, the geometry will gradually become less visible in the distance approaching the fog color.

## Alpha Test

During the **Alpha Test** stage, fragments can be discarded if the alpha value is below a certain threshold. This stage only functions if the **GL_ALPHA_TEST** is enabled and the framebuffer uses a color mode that stores the alpha value (such as RGBA).

## Depth & Stencil

During the **Depth** test stage, a fragment can be discarded if the current framebuffer has a depth buffer, and the depth test stage (**GL_DEPTH_TEST**) is enabled and it fails the depth comparison function [[6\]](https://www.3dgep.com/introduction-to-opengl-and-glsl/#Depth%20Buffer%20Test). By default, a fragment will be discarded if its depth value is greater than the depth value of the current fragment (that is, it is behind the current fragment).

The stencil test can be used to discard fragments that fail a stencil comparison operation based on the content of the stencil buffer [[7\]](https://www.3dgep.com/introduction-to-opengl-and-glsl/#Stencil%20test).

## Color Buffer Blend

If color blending is enabled (glEnable(**GL_BLEND**)) then blending will be performed based on the blend function and the blend equations. Fragments that are currently being rendered are referred to as the “source” and fragments that have already been written to the framebuffer are referred to as the “destination”. Blending is performed by combining the colors and alpha values from the source fragment with the destination fragment.

## Dither

If you are using a color palette with few colors, you can enable the **GL_DITHER** state to inform OpenGL to try to simulate a larger color palette by mixing colors in close proximity. This is similar to the effect you get when you are using an indexed image compression algorithm such as Graphics Interchange Format [[8\]](https://www.3dgep.com/introduction-to-opengl-and-glsl/#Graphics%20Interchange%20Format) (GIF) which uses a color palette with few colors.

# OpenGL 2.0 Programmable Pipeline

The stages of the fixed-function rendering pipeline were described in the previous section. With the release of OpenGL 2.0 in 2004 the ability to programmatically define the vertex transformation and lighting (T&L) and the fragment operations were introduced. The stages colored green in the fixed-function pipeline are replaced by the vertex program and fragment program. The resulting pipeline now has the following stages.

![OpenGL 2.0 Pipeline](https://www.3dgep.com/wp-content/uploads/2014/02/OpenGL-2.0-Pipeline.png)

OpenGL 2.0 Pipeline

## Vertex Shader

The previously defined fixed-function transformation and lighting stage are now replaced by the programmable vertex shader program. This gives the graphics programmer more flexibility regarding how the vertices are transformed (we can even decide not to transform the vertices at all) and we can even perform the lighting computations in the fragment shader to achieve per-pixel lighting (as opposed to per-vertex which was a limitation of the fixed-function pipeline).

However with great power, comes great responsibility. The fixed-function pipeline performs a lot of the operations that beginning graphics programmers will have difficulty with (for example, the vertex transformations are always a difficult concept to master). Throughout this article, we will learn how to use the programmable shader pipeline to define a vertex program that can be used to transform the vertex data into the correct space for rasterization.

The primary responsibility of the vertex shader program is to transform the vertex position into “clip space”. Commonly, this is done by multiplying the vertex position by the model-view-projection matrix (commonly referred to as the MVP matrix). I will discuss this process in detail later in the article.

## Fragment Shader

The fragment shader program replaces all of the complicated texture blending, color sum, and fog operations from the fixed-function pipeline. The graphics programmer must create a fragment program that performs these operations (or not if you don’t care about the distance fog for example).

The primary responsibility of the fragment program is to determine the final color (or colors in the case of multiple color targets) of the fragment. The fragment program can be used to compute the per-pixel lighting as well as blend together multiple textures to determine the final fragment color.

# OpenGL 3.2 Programmable Pipeline

OpenGL 3.2 (released in 2009) introduced an additional stage to the programmable shader pipeline called the geometry shader [[9\]](https://www.3dgep.com/introduction-to-opengl-and-glsl/#History%20of%20OpenGL). The OpenGL 3.2 pipeline looks like this:

![OpenGL 3.2 Pipeline](https://www.3dgep.com/wp-content/uploads/2014/02/OpenGL-3.2-Pipeline.png)

OpenGL 3.2 Pipeline

## Geometry Shader

The geometry shader comes after the vertex shader in the programmable shader pipeline and therefore the output of the vertex shader becomes the input to the geometry shader. The geometry shader can be used to process primitives of one type and generate primitives of another type. For example, you could pass a stream of points (a single vertex) that represents the 3D positions of particles in a particle system and the geometry shader can produce a set of triangles that can be used to render the texture mapped, orientated particles.

# OpenGL 4.0 Programmable Pipeline

OpenGL 4.0 (released in 2010) introduced a set of three additional stages to the programmable shader pipeline. These stages come after the Vertex Shader but before the Geometry Shader. These three new stages form the Tesselation stages.

![OpenGL 4.0 Pipeline](https://www.3dgep.com/wp-content/uploads/2014/02/OpenGL-4.0-Pipeline-1024x332.png)

OpenGL 4.0 Pipeline

The three parts of the tessellation stages are the **Tesselation Control Shader** (TCS), the **Tessellator** (tessellation primitive generator), and the **Tessellation Evaluation Shader** (TES) [[10\]](https://www.3dgep.com/introduction-to-opengl-and-glsl/#Tesselation). From these three stages, the TCS and the TES are programmable. The Tessellator is a fixed-function stage and cannot be programmed.

The tessellation stages do not function on the classic OpenGL primitive types (GL_POINTS, GL_LINES, GL_TRIANGLES, etc.) but instead function on patches (**GL_PATCH**) primitive type. Patches are simply a stream of vertices that are to be considered a single patch primitive. A patch can consist of 3 vertices in which case the patch is simply a triangle but a patch can also consist of 4 or more vertices (polygon tesselation). The number of vertices that define a single patch in OpenGL is defined by the **glPatchParameteri** function. This function takes 2 parameters. The first parameter must be **GL_PATCH_VERTICES** and the second parameter is an integer value that determines the number of vertices in the vertex stream that are to be considered a patch. By default, this value is 3.

## Tessellation Control Shader

The **Tesselation Control Shader** (TCS) is an optional shader stage (it is not required to specify a shader program for this stage). The primary responsibility of the TCS is to determine how much to tessellate the output patch [[1\]](https://www.3dgep.com/introduction-to-opengl-and-glsl/#OpenGL%20Programming%20Guide).

## Tesselator

The **Tessellator** (or Tessellation primitive generator) is a fixed stage in the rendering pipeline. It consumes the output from the TCS and creates new tessellated primitives. It passes the tessellated coordinates to the TES stage (described next) for further processing [[1\]](https://www.3dgep.com/introduction-to-opengl-and-glsl/#OpenGL%20Programming%20Guide) [[10\]](https://www.3dgep.com/introduction-to-opengl-and-glsl/#Tessellation).

The purpose of the tessellator is to determine how many vertices to generate, in which order to generate them, and what kind of primitives to build out of them [[10\]](https://www.3dgep.com/introduction-to-opengl-and-glsl/#Tessellation).

## Tessellation Evaluation Shader

The **Tessellation Evaluation Shader** (TES) is invoked for each tessellation coordinate generated by the tessellation primitive generator (the previous stage). The primary purpose of the TES is to determine the final attributes of each of the tessellated vertices. This is the only required shader stage that must be present for tessellation to occur.

# Dependencies

Before we start setting up the project, we need to obtain a few dependencies that we will be using to facilitate the creation of the OpenGL application.

1. [Free OpenGL Utility Toolkit (FreeGLUT 2.8.1)](http://freeglut.sourceforge.net/): FreeGLUT is an open-source alternative to the OpenGL Utility Toolkit (GLUT) library. The purpose of the GLUT library is to provide window, keyboard, and mouse functionality in a platform-independent way.
2. [OpenGL Extension Wrangler (GLEW 1.10.0)](http://glew.sourceforge.net/): The OpenGL Extension Wrangler library provides accessibility to the OpenGL core and extensions that may not be available in the OpenGL header files and libraries provided by your compiler.
3. [OpenGL Math Library (GLM 0.9.5)](http://glm.g-truc.net/0.9.5/index.html): The OpenGL math library is written to be consistent with the math primitives provided by the GLSL shading language. This makes writing math functions in C++ very similar to the math functions you will see in GLSL shader code.

These dependencies are all platform agnostic. They should work on Mac, Windows, and Unix/Linux based operating systems.

# Setting Up the Project

In the next sections, I will step through the process of creating a project in Visual Studio 2012 that is suitable for creating OpenGL applications. If you are already very familiar with setting up projects and configuring dependencies, feel free to skip to the next section titled [A Simple OpenGL Shader Program](https://www.3dgep.com/introduction-to-opengl-and-glsl/#A_Simple_OpenGL_Shader_Program)

In this article, I will only show how to create a minimal shader program that consits of a simple vertex shader and a fragment shader. I will step through the different stages of the programmable shader pipeline to see how we can use OpenGL to push vertices through the rendering pipeline to generate the final rendered image. The steps of the rendering pipeline that we are concerned with are:

1. **Vertex Data**: Specify the vertex data that represents the model that we want to render.
2. **Primitive Processing**: Specify the primitive type that is used by our vertex data.
3. **Vertex Shader**: Create a simple vertex shader that will transform the vertices from object-space to clip-space.
4. **Fragment Shader**: Create a simple fragment shader that will determine the final color of the fragment that will appear on screen.

## The OpenGL Project

For this tutorial, I will be using Visual Studio 2012 to create a simple OpenGL demo. All of the source code should be platform agnostic so you should be able to follow the tutorial if you are programming for Linux, Mac OS, or any other desktop platform.

Let’s start Visual Studio 2012 and create a new project.

![Visual Studio 2012](https://www.3dgep.com/wp-content/uploads/2014/01/Visual-Studio-2012-1024x620.png)

Visual Studio 2012

In the New Project dialog box, select the **Win32 Console Application**. Specify a name, location, and solution name for your new project.

![Visual Studio 2012 - New Project Dialog](https://www.3dgep.com/wp-content/uploads/2014/01/Visual-Studio-2012-New-Project-Dialog.png)

Visual Studio 2012 – New Project Dialog

In the **Win32 Application Wizard** dialog box, click **Next**.

![Visual Studio 2012 - Win32 Application Wizard](https://www.3dgep.com/wp-content/uploads/2014/01/Visual-Studio-2012-Win32-Application-Wizard.png)

Visual Studio 2012 – Win32 Application Wizard

In the **Win32 Application Wizard – Application Settings** dialog box, select the **Empty project** check box and click the **Finish** button.

![Visual Studio 2012 - Win32 Application Wizard (2)](https://www.3dgep.com/wp-content/uploads/2014/01/Visual-Studio-2012-Win32-Application-Wizard-2.png)

Visual Studio 2012 – Win32 Application Wizard (2)

You should now have an empty project that we can start creating our first OpenGL application in.

![Visual Studio 2012 - OpenGL Project](https://www.3dgep.com/wp-content/uploads/2014/01/Visual-Studio-2012-OpenGL-Project-1024x550.png)

Visual Studio 2012 – OpenGL Project

## Project Setup

Let’s start by adding a new source file to our new project. Right-click the project in the **Solution Explorer** and select **Add > New Item…** from the pop-up menu that appears.

From the dialog box that appears, select the **C++ File (.cpp)** template from the **Visual C++ > Code** template category.

Specify the name **main** and put the file in the **src** folder relative to your project.

![Visual Studio 2012 - Add New Item](https://www.3dgep.com/wp-content/uploads/2014/02/Visual-Studio-2012-Add-New-Item.png)

Visual Studio 2012 – Add New Item

Before we start coding, let’s setup the project correctly to facilitate good development practices.

In the folder where the **project file** is located create the following folders:

- **src**: This folder will be used to store the “private” source files for our project.
- **inc**: This folder will be used to store the “public” header files for our project.
- **bin**: This folder will be used to store the compiled executable files.

Open the project properties dialog box and change the **Output Directory** to **bin\** for both the **Debug** and **Release** build configurations.

Change the **Target Name** to **$(ProjectName)d** for the **Debug** build configuration and leave it as **$(ProjectName)** for the **Release** build configuration. The “d” at the end of the Target Name property will indicate that it is a debug build.

[![Visual Studio 2012 - Project Properties](https://www.3dgep.com/wp-content/uploads/2014/01/Visual-Studio-2012-Project-Properties.png)](https://www.3dgep.com/wp-content/uploads/2014/01/Visual-Studio-2012-Project-Properties.png)

We also need to tell Visual Studio to use the bin folder as the Working Directory when debugging the application.

In the **Debugging** properties, change the **Working Directory** to **$(OutDir)** for both the **Debug** and **Release** build configurations.

![Visual Studio 2012 - Project Properties (2)](https://www.3dgep.com/wp-content/uploads/2014/01/Visual-Studio-2012-Project-Properties-2.png)

Visual Studio 2012 – Project Properties (2)

## Adding Dependencies

We also want to place the dependencies in a location that can be accessed by several projects in our solution. Create a folder called **extern** in the same folder as the solution file. We will place all the external dependencies in this folder. Placing external dependencies in a folder with your solution file makes it easy to share and distribute your solution files with others later on.

As stated earlier, we have three dependencies: FreeGLUT, GLEW, and GLM.

### FREEGLUT

Download the latest FreeGLUT libraries from <http://freeglut.sourceforge.net/>. At the time of this writing, the latest version of FreeGLUT was 2.8.1.

Extract the contents of the zip file to the **extern** folder.

### GLEW

Download the latest source zip GLEW libraries from <http://glew.sourceforge.net/>. At the time of this writing, the latest version of the GLEW library was 1.10.0.

Extract the contents of the zip file to the **extern** folder.

### GLM

Download the latest version of the GLM library from <http://glm.g-truc.net/>. A the time of this writing, the latest version of the GLM library was 0.9.5.1.

Extract the contents of the zip file to the **extern** folder and change the name of the glm folder to **glm-0.9.5.1** to reflect the version of the library that you are using.

You should now have the following folders in the **extern** folder:

- freeglut-2.8.1
- glew-1.10.0
- glm-0.9.5.1

Of course the version numbers may vary depending on the versions you downloaded.

## Setup Dependencies

We need to update the project settings so that Visual Studio knows where to find the include files and compiled library files.

In the project’s property dialog, add the following paths to the **Additional Include Directories** property for all build configurations:

- inc
- ..\extern\freeglut-2.8.1\include
- ..\extern\glew-1.10.0\include
- ..\extern\glm-0.9.5.1

![Visual Studio 2012 - Project Properties (3)](https://www.3dgep.com/wp-content/uploads/2014/01/Visual-Studio-2012-Project-Properties-3.png)

Visual Studio 2012 – Project Properties (3)

Folder paths should always be specified relative to the project file. You should never use absolute paths (paths that start with a drive letter) and you should not use the $(SolutionDir) property to specify paths. The reason for this is portability. If you want to distribute your source code and project files to someone else then the relative paths will still be correct but absolute paths will not be the same on another computer. It should also be possible to use your projects in multiple solution files. If the solution files are located in different folders then the project-relative paths will still be correct but the ​$(SolutionDir) variable will be different.

The FreeGLUT and the GLEW library need to be compiled but the GLM library does not require any compilation (GLM is a header-only library).

## Setup FreeGLUT Dependencies

FreeGLUT provides Visual Studio 2012 project files so we can add this project to our solution directly.

Add an existing project to your solution and select the FreeGLUT Visual Studio 2012 project file located in the “**extern\freeglut-2.8.1\VisualStudio\2012**” folder.

![Visual Studio 2012 - Solution Explorer](https://www.3dgep.com/wp-content/uploads/2014/01/Visual-Studio-2012-Solution-Explorer.png)

Visual Studio 2012 – Solution Explorer

FreeGLUT provides both static and dynamic linking configurations. I always prefer to use static linking for all my projects. If you prefer to use dynamic linking then you must also copy the DLL files to the bin folder where your executable resides. If you use static linking, you do not need to distribute any additional DLL files with your executable.

To make sure FreeGLUT builds the static libraries, open the build Configuration Manager (**Build > Configuration Manager…** from the main menu).

Set the **Active solution configuration** to **Debug** and the **Active solution platform** to **Win32**. Make sure the **freeglut** project is configured to build the **Debug_Static** configuration.

![Visual Studio 2012 - Configuration Manager](https://www.3dgep.com/wp-content/uploads/2014/01/Visual-Studio-2012-Configuration-Manager.png)

Visual Studio 2012 – Configuration Manager

Do the same for the **Release** solution configuration but make sure the configuration for the freeglut project is set to **Release_Static**.

![Visual Studio 2012 - Configuration Manager (2)](https://www.3dgep.com/wp-content/uploads/2014/01/Visual-Studio-2012-Configuration-Manager-2.png)

Visual Studio 2012 – Configuration Manager (2)

Optionally, you can also edit the build configurations and remove configurations that you do not want to see in the Build and Platform drop-down menus in Visual Studio.

Build the FreeGLUT libraries for both the **Release** and **Debug** build configurations by right-clicking on the **freeglut** project in the solution explorer and selecting **Build** from the pop-up menu that appears.

![Visual Studio 2012 - Build Freeglut](https://www.3dgep.com/wp-content/uploads/2014/02/Visual-Studio-2012-Build-Freeglut.png)

Visual Studio 2012 – Build Freeglut

When we build the solution, want to make sure that the FreeGLUT binaries are built before our own application is built. To do that we can create a dependency between our project and the FreeGLUT project.

Open the **Project Dependencies** dialog by selecting **Project > Project Dependencies…** from the main menu.

In the **Project Dependencies** dialog box select your project from the **Projects** drop-down. Check the box next to the **freeglut** project in the **Depends on** list.

![Visual Studio 2012 - Project Dependencies](https://www.3dgep.com/wp-content/uploads/2014/01/Visual-Studio-2012-Project-Dependencies.png)

Visual Studio 2012 – Project Dependencies

One last step is to configure our project to link against the FreeGLUT static libraries.

Open the Project Properties dialog for your project and select the **Linker > General** properties.

Add the following paths to the **Additional Library Directories** property:

The library paths may not be present until you have built the freeglut library files for both the Debug and Release configurations.

For the **Debug** configuration:

- ..\extern\freeglut-2.8.1\lib\x86\Debug

![Visual Studio 2012 - Linker Properties (1)](https://www.3dgep.com/wp-content/uploads/2014/02/Visual-Studio-2012-Linker-Properties-1.png)

Visual Studio 2012 – Linker Properties (1)

For the **Release** configuration:

- ..\extern\freeglut-2.8.1\lib\x86

![Visual Studio 2012 - Linker Properties (2)](https://www.3dgep.com/wp-content/uploads/2014/02/Visual-Studio-2012-Linker-Properties-21.png)

Visual Studio 2012 – Linker Properties (2)

For both the **Debug** and **Release** configurations, in the **Linker > Input** property sheet, add the library files to to the **Additional Dependencies** property:

- freeglut_static.lib

![Visual Studio 2012 - Linker Properties (3)](https://www.3dgep.com/wp-content/uploads/2014/02/Visual-Studio-2012-Linker-Properties-3.png)

Visual Studio 2012 – Linker Properties (3)

## Setup GLEW Dependencies

Now we are going to add the dependencies for the GLEW project.

Unlike Freeglut, GLEW does not provide project files directly for Visual Studio 2012. But we can use the supplied project files for Visual Studio 2010 and just change the toolchain to compile with Visual Studio 2012.

Add the **glew_static.vcxproj** project file from the **extern\glew-1.10.0\build\vc10** directory into your solution.

![Visual Studio 2012 - Add Glew Project](https://www.3dgep.com/wp-content/uploads/2014/02/Visual-Studio-2012-Add-Glew-Project.png)

Visual Studio 2012 – Add Glew Project

You may notice that the project gets added with the name **glew_static (Visual Studio 2010)**. This name indicates that this project was intended to be used with the Visual Studio 2010 IDE but we are including it in a Visual Studio 2012 IDE. To fix this, we can simply update the project to use the Visual Studio 2012 tool chain.

Right-click on the **glew_static** project and select **Properties** from the pop-up menu that appears.

Make sure you select **All Configurations** from the **Configurations** drop-down menu.

In the **Configuration Properties > General** property tree change the **Platform Toolset** property to **Visual Studio 2012 (v110)**.

![Visual Studio 2012 - GLEW Project Properties](https://www.3dgep.com/wp-content/uploads/2014/02/Visual-Studio-2012-GLEW-Project-Properties.png)

Visual Studio 2012 – GLEW Project Properties

Build the GLEW library binaries by right-clicking on the project and select **Build** from the pop-up menu that appears.

You may get the following error when you try to build the GLEW project:

```
`1>------ Build started: Project: glew_static, Configuration: Release Win32 ------``1>  glew.c``1>..\glew.rc(59): error RC2102: string literal too long``1>  ``========== Build: 0 succeeded, 1 failed, 0 up-to-date, 0 skipped ==========`
```

To fix this error, open the **glew.rc** file by double-clicking on the error in the **Output** window.

You will notice that the string on line 59 of this file is very long. To fix this, simply scroll to somewhere near the middle of the string and add a break by adding a pair of double quotes separated by a space to break the single string value into 2 separate string values (as shown in the highlighted text below).

![Glew rc - Fix String Literal Too Long](https://www.3dgep.com/wp-content/uploads/2014/02/Glew-rc-Fix-String-Literal-Too-Long.png)

Glew rc – Fix String Literal Too Long

Save the file and try to build the glew_static project again. It should build now without errors. Make sure you can build both the **Debug** and **Release** build configurations.

To ensure that the **glew_static** project is built before our own project, we can create a dependency between the **glew_static** project and ours.

Select **Project > Project Dependencies…** from the main menu.

From the **Projects** pull-down menu, make sure you select the **Tutorial1** project is selected (or whatever name you chose for your own project).

Make sure the check box for both **freeglut** and **glew_static** projects are selected.

![Visual Studio 2012 - Project Dependencies (2)](https://www.3dgep.com/wp-content/uploads/2014/02/Visual-Studio-2012-Project-Dependencies-2.png)

Visual Studio 2012 – Project Dependencies (2)

Doing this guarantees that Visual Studio will not try to compile our project unless all of it’s dependencies are built first.

In a previous step, we have already added the references to the include files. At this point we only need to tell Visual Studio to link in the library dependencies for the **glew_static** library.

Open the project properties dialog for the application project (Tutorial1 in my case).

In the **Linker > General** property sheet, add the paths to the **Additional Library Directories** property:

Debug:

- ..\extern\glew-1.10.0\lib\Debug\Win32

![Visual Studio 2012 - Linker Properties (4)](https://www.3dgep.com/wp-content/uploads/2014/02/Visual-Studio-2012-Linker-Properties-4.png)

Visual Studio 2012 – Linker Properties (4)

Release:

- ..\extern\glew-1.10.0\lib\Release\Win32

![Visual Studio 2012 - Linker Properties (5)](https://www.3dgep.com/wp-content/uploads/2014/02/Visual-Studio-2012-Linker-Properties-5.png)

Visual Studio 2012 – Linker Properties (5)

In the **Linker > Input** property sheet, add the library files to to the **Additional Dependencies** property:

Debug:

- glew32sd.lib

Release:

- glew32s.lib

![Visual Studio 2012 - Linker Properties (6)](https://www.3dgep.com/wp-content/uploads/2014/02/Visual-Studio-2012-Linker-Properties-6.png)

Visual Studio 2012 – Linker Properties (6)

## Setup GLM Dependencies

There is very little setup to perform for the GLM math library because it is a header-only library. The only thing you must make sure you do is configure the include paths correctly (which we have already earlier).

One nice feature that GLM provides is visualizers that tell Visual Studio how to display the vector and matrix types in the debugger. To use the visualizers provided by GLM, copy the **extern\glm-0.9.5.1\util\glm.natvis** file to the **Documents\Visual Studio 2012\Visualizers** folder in your personal user folder.

Optionally, you can also copy the **extern\glm-0.9.5.1\util\usertype.dat** file to the **C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE** folder (if the file already exists, you may want to merge the contents of this file with the one already there). This file tells visual studio which types should be highlighted in the editor.

Enough with setting up dependencies, let’s start writing some code!

# A Simple OpenGL Shader Program

For this introduction demo, we’re simply going to make a rotating cube. This will be a very simple example and in the end you should know how to load geometry into OpenGL and render that geometry using a simple vertex and fragment program.

## Global Header File

First, we’ll start off by making a global header file that can be included in all of our source files. For this project, we only have 1 or 2 source files but it is generally a good idea to put all of the header files for external projects (like Freeglut and GLUT) and system includes (like STL containers) in a “global” header file. Doing this has several advantages:

1. You usually only have to include a single header file to get consistent functionality throughout all of your CPP files.
2. You should not include 3rd party headers and system header files in your local project’s header files. Doing so may cause inconsistent behavior (for example if you need to declare specific macros before the inclusion of 3rd party header files, keeping everything in a single “global” header file will ensure consistency).
3. Having a single global header file facilitates the use of precompiled headers. See [Creating Precompiled Header Files](https://msdn.microsoft.com/en-us/library/szfdksca(v=vs.110).aspx) for more information.

You should not include any “local” header files in this include file. Local header files for your project tend to change frequently which defeats the purpose of using precompiled headers (because it will cause the precompiled header to be rebuilt frequently). The main advantage of precompiled headers is that they only have to be prebuilt once and can be quickly included in subsequent builds.

To create the “global” header file, create a new header file in your project called **inc\Tutorial1PCH.h** for example. The **PCH** acronym appended to the end of the name implies that this is a **p**re**c**ompiled **h**eader. Visual Studio will not automatically treat this header file like a precompiled header until you configure your project to use precompiled headers. Configuring the project to use precompiled headers is beyond the scope of this article.

Open the **Tutorial1PCH.h** header file in the editor and add the following lines:

```
`#include <string>``#include <iostream>``#include <fstream>``#include <vector>``#include <ctime>` `#define GLEW_STATIC``#include <GL/glew.h>``#include <GL/wglew.h> // For wglSwapInterval` `#define FREEGLUT_STATIC``#include <GL/freeglut.h>` `#define GLM_FORCE_RADIANS``#include <glm/glm.hpp>``#include <glm/gtc/type_ptr.hpp>``#include <glm/gtx/transform.hpp>``#include <glm/gtx/quaternion.hpp>`
```

The first five includes are a few headers from the STL. We’ll be using file streams to read the shader files so we need the **fstream** header. The **ctime** header adds functions for reading the system clock. This will be useful for computing the amount of time that has elapsed between frame updates.

The **GLEW_STATIC** macro definition is required when we are linking against the GLEW static library. The **wglew.h** is required for access to the **wglSwapInterval** function. This function is useful for disabling vsync and allows our program to run as fast as it can (instead of waiting for the vertical refresh which is usually 60 Hz. Without disabling vsync, your program would never run faster than the refresh rate of your monitor).

It is useful to note that the **wglew.h** header file includes OpenGL extensions that are specific to Windows only. If you intend to port this code to another platform it would be necessary to guard the inclusion of this file with the existence of the **_WIN32** macro:

```
`#ifdef _WIN32``#include <GL/wglew.h> // For wglSwapInterval``#endif`
```

Using this technique will ensure this header file is only included when compiling on Windows.

It is also important to note that the GLEW header files must be included before the freeglut headers. GLEW requires that it is included before the OpenGL header files. The FreeGLUT headers will include the OpenGL headers so it is important to always put the GLEW headers before the FreeGLUT headers.

The **FREEGLUT_STATIC** macro definition is required when linking with the FreeGLUT static library. If you look into the FreeGLUT header file, you may notice that the **freeglut_static.lib** library file will be automatically linked into our application if we declare the **FREEGLUT_STATIC** macro before the header file:

```
`#pragma comment (lib, "freeglut_static.lib")`
```

This implies that we don’t need to explicitly specify this file in the linker settings but I prefer to be explicit about which files my project is linking against in the project settings. However this statement is a bit contradictory because I don’t explicitly specify the OpenGL libraries which are auto linked:

```
`/* Drag in other Windows libraries as required by FreeGLUT */``#   if FREEGLUT_LIB_PRAGMAS``#       pragma comment (lib, "glu32.lib")    /* link OpenGL Utility lib     */``#       pragma comment (lib, "opengl32.lib") /* link Microsoft OpenGL lib   */``#       pragma comment (lib, "gdi32.lib")    /* link Windows GDI lib        */``#       pragma comment (lib, "winmm.lib")    /* link Windows MultiMedia lib */``#       pragma comment (lib, "user32.lib")   /* link Windows user lib       */``#   endif`
```

By the way, you may have noticed that we don’t need to install any dependencies for OpenGL itself. This is because the OpenGL 1.1 core header and libraries are included with your compiler. These headers and libraries are part of the Windows SDK and are automatically included in projects created with Visual Studio. But since the OpenGL header files that come with your compiler only define the OpenGL 1.1 core specification, we need a library like GLEW to access the new functions and features of the OpenGL SDK.

The **GLM_FORCE_RADIANS** macro definition is required to ensure that all of the GLM functions consistently use radians instead of degrees for all of the math functions. In the past, GLM had a very inconsistent usage of angle units. This inconsistency is addressed by this macro and in later versions of GLM this macro will no longer be required because radians will be the default behavior for the GLM math library. So if you are using GLM 0.9.6.0 or higher (which was not yet available at the time of this writing) you may not need to speicfy the **GLM_FORCE_RADIANS** macro.

## The Main Source

Now that we have our global header file defined, lets get to work on the main application code.

Clearly, the first thing we need is the global header file:

```
`#include <Tutorial1PCH.h>`
```

I am also using a simple camera class for this demo. This class simplifies some of the functionality to move and rotate the view and compute the view matrix and projection matrix that are required to render the 3D scene. You can download the files for the camera class here:

[![img](https://www.3dgep.com/wp-content/uploads/2013/10/zip-icon_03-e1382533375997.png)Camera.zip](https://drive.google.com/file/d/0B0ND0J8HHfaXUFlVVk15LUNzbjQ/edit?usp=sharing)



Download and extract the **Camera.zip** file to your project folder and include the header and source files in your project in Visual Studio. You may need to change the name of the global header file in the **Camera.cpp** file to match the name of the global include file used in your project.

Include the camera’s header file in your main source:

```
`#include <Camera.h>`
```

Next, let’s define some macros that can be used later when setting up our vertex buffers.

```
`#define BUFFER_OFFSET(offset) ((void*)(offset))``#define MEMBER_OFFSET(s,m) ((char*)NULL + (offsetof(s,m)))`
```

I’ll explain the purpose of these macros later when they are used in the code.

Next, we’ll define a few global variables that are used by our application.

```
`int` `g_iWindowWidth = 800;``int` `g_iWindowHeight = 600;``int` `g_iWindowHandle = 0;` `int` `g_W, g_A, g_S, g_D, g_Q, g_E;``bool` `g_bShift = ``false``;` `glm::ivec2 g_MousePos;` `glm::quat g_Rotation;` `std::``clock_t` `g_PreviousTicks;``std::``clock_t` `g_CurrentTicks;` `Camera g_Camera;``glm::vec3 g_InitialCameraPosition;``glm::quat g_InitialCameraRotation;`
```

The first two variables will define how large our render window will be. The **g_iWindowHandle** variable is used to store the ID of the window which gets created by FreeGLUT. We will need to keep track of the unique window IDs if we have more than one window. In this application, we will only create a single window.

The **g_W**, **g_A**, **g_S**, **g_D**, **g_Q**, and **g_E** integer variables will be set to 1 if the corresponding key on the keyboard is pressed. Otherwise, these variables will be set to 0 if the corresponding key on the keyboard is not pressed. We will use these values to pan the camera around the scene. Likewise, the **g_bShift** boolean variable will be set to **true** if either the left shift or right shift keys are pressed.

The **g_MousePos** variable will be used to keep track of the mouse position on screen. This is required to correctly compute how far the mouse has moved between frames.

The **g_Rotation** variable is a quaternion that is used to store the rotation of the object in the scene. If you are not familiar with quaternions, I suggest you read my article title [Understanding Quaternions](https://www.3dgep.com/?p=1815).

The **g_PreviousTicks** and **g_CurrentTicks** variables will be used to compute the amount of time that has elapsed between frames. This is useful for creating a consistent user experience regardless of the frame-rate on the end-users computer.

We’ll also use the **Camera** class that you should have downloaded earlier. We’ll first declare a camera object and a few parameters that will be used to reset the camera’s view to the default position and orientation when the user pressed the ‘R’ key on the keyboard.

For this demo, we will render a simple colored cube. Before we can render the cube, we need to define the geometry for the cube. Since creating any kind of model loader is beyond the scope of this article, I will just define the geometry in-line using static arrays.

First we need to define a single vertex of our geometry. Each vertex of our geometry will consist of a position attribute and a color attribute. We will store each of these attributes as 3-component vectors. Lets define a struct that can be used to store the vertex data.



```
`struct` `VertexXYZColor``{``    ``glm::vec3 m_Pos;``    ``glm::vec3 m_Color;``};`
```

Here we define a struct called **VertexXYZColor** which has 2 member variables:

1. **glm::vec3 m_Pos**: This variable is used to store the position of the vertex in 3-dimensional space.
2. **glm::vec3 m_Color**: This variable stores the Red, Green, and Blue color components for our vertices. In this demo we will not be doing any blending so we will neglect the alpha value.

Next we will define the 8 unique vertices for our cube.

```
`// Define the 8 vertices of a unit cube``VertexXYZColor g_Vertices[8] = {``    ``{ glm::vec3(  1,  1,  1 ), glm::vec3( 1, 1, 1 ) }, ``// 0``    ``{ glm::vec3( -1,  1,  1 ), glm::vec3( 0, 1, 1 ) }, ``// 1``    ``{ glm::vec3( -1, -1,  1 ), glm::vec3( 0, 0, 1 ) }, ``// 2``    ``{ glm::vec3(  1, -1,  1 ), glm::vec3( 1, 0, 1 ) }, ``// 3``    ``{ glm::vec3(  1, -1, -1 ), glm::vec3( 1, 0, 0 ) }, ``// 4``    ``{ glm::vec3( -1, -1, -1 ), glm::vec3( 0, 0, 0 ) }, ``// 5``    ``{ glm::vec3( -1,  1, -1 ), glm::vec3( 0, 1, 0 ) }, ``// 6``    ``{ glm::vec3(  1,  1, -1 ), glm::vec3( 1, 1, 0 ) }, ``// 7``};`
```

The order that these vertices appear in the vertex array are completely arbitrary. The order that the vertices will be sent to the GPU is determined by another buffer called the Index buffer which we will declare next.

```
`// Define the vertex indices for the cube.``// Each set of 6 vertices represents a set of triangles in ``// counter-clockwise winding order.``GLuint g_Indices[36] = {``    ``0, 1, 2, 2, 3, 0,           ``// Front face``    ``7, 4, 5, 5, 6, 7,           ``// Back face``    ``6, 5, 2, 2, 1, 6,           ``// Left face``    ``7, 0, 3, 3, 4, 7,           ``// Right face``    ``7, 6, 1, 1, 0, 7,           ``// Top face``    ``3, 2, 5, 5, 4, 3            ``// Bottom face``};`
```

Since OpenGL only knows about a few basic primitive types (GL_POINTS, GL_LINES, and GL_TRIANGLES) and variations thereof, we cannot simply send a set of 8 vertices to the GPU and expect that it knows how to construct a solid cube out of them. Well, technically we could send 36 vertices to the GPU without using an index buffer but the vertices would have to be duplicated several times to properly define the cube. Instead of creating a lot of duplicate data, we can specify an index buffer that determines the order in which to push the vertices to the GPU.

Vertices can only be shared this way using an index buffer if all of the vertex attributes are the same. If we introduced vertex normals, then only one face of the cube would have the correct normals applied. To resolve this issue, we would need to declare a unique vertex for each of the 24 unique vertices of the cube.

We must be careful regarding the order in which the vertices are sent to the GPU. As an optimization OpenGL can remove primitives from the rendering pipeline which are not facing the camera. Whether a primitive is facing the camera or not is determined by its winding order. The default winding order of front-facing triangles is counter-clockwise as shown in the image below.

![Vertex Winding Order](https://www.3dgep.com/wp-content/uploads/2011/03/Terrain-vertex-winding-order.gif)

Vertex Winding Order

Each face of the cube consists of two triangles; an upper triangle and a lower triangle. In previous versions of OpenGL, we could have used quads to represent a single face of the cube but quads have been deprecated in OpenGL 3.1 and removed in OpenGL 3.2. Each triangle must be specified in a counter-clockwise winding order.

Next we need to define a few variables to store handles to the objects that will be created by OpenGL.

```
`// Vertex array object for the cube.``GLuint g_vaoCube = 0;``GLuint g_ShaderProgram = 0;``// Model, View, Projection matrix uniform variable in shader program.``GLint g_uniformMVP = -1;`
```

The **g_vaoCube** variable will be used to refer to the **Vertex Array Object** that will be used to render our cube. A **Vertex Array Object** can be used to bind all of the vertex attributes and the index buffer into a single argument. Using the **Vertex Array Object** will greatly simplify the display function.

The **g_ShaderProgram** variable will be used to store the reference to the compiled and linked shader program. The shader program combines both the vertex shader and the fragment shader into a single object. You can think of the shader program similar to an application that runs on the GPU instead of on the CPU. The individual shaders (vertex, fragment) first need to be compiled into a form of object code (similar to those .obj files generated by your compiler). The compiled shaders are then linked together into a final shader program which can be executed on the GPU.

The **g_uniformMVP** variable is used to reference another variable that is defined in the shader program. The **MVP** acronym suggests that the shader variable defines the concatentated model-view-projection matrix that is used to transform the cube’s vertices into clip-space (more about transformation and spaces can be found in another article titled [Transformation and Lighting](https://www.3dgep.com/?p=2828#Transformations_and_the_Importance_of_Spaces)).

Next, we will declare some functions that will be used as callbacks for windowing events (keyboard, mouse, rendering, and idle).

```
`void IdleGL();``void DisplayGL();``void KeyboardGL( unsigned ``char` `c, ``int` `x, ``int` `y );``void KeyboardUpGL( unsigned ``char` `c, ``int` `x, ``int` `y );``void SpecialGL( ``int` `key, ``int` `x, ``int` `y );``void SpecialUpGL( ``int` `key, ``int` `x, ``int` `y );``void MouseGL( ``int` `button, ``int` `state, ``int` `x, ``int` `y );``void MotionGL( ``int` `x, ``int` `y );``void ReshapeGL( ``int` `w, ``int` `h );`
```

The **IdleGL** function will be called whenever no other windowing events need to be handled. We can use this function to update the “game” logic.

The **DisplayGL** function will be called whenever the contents of the window need to be redrawn (for example, if something in the scene moves or the window is resized).

The **KeyboardGL** function will be called if a key is pressed on the keyboard. This function will be called repeatedly if the key is held down.

The **KeyboardUpGL** function will be called when a key is released on the keyboard.

The **SpecialGL** function will be called when a non-printable character (like the shift, control, alt, or arrow keys) is pressed on the keyboard. This function will be called repeatedly if the key is held down.

And similarly, the **SpecialUpGL** will be called when a non-printable character key is released.

The **MouseGL** function will be called when a mouse button is pressed while the mouse cursor is over the window.

The **MotionGL** function will be called when the mouse is dragged over the window. We will use this function to rotate the cube.

The **ReshapeGL** function will be called when the window is resized.

Before we start defining the functions, let’s first create the vertex and fragment shaders that we will load.

## Shaders

For this demo, we will use two shaders. A **vertex** shader and a **fragment** shader. These are the minimum shaders that are required to render something on the screen using OpenGL (unless we decide to keep using the deprecated fixed-function pipeline, in that case we don’t need any shaders).

The primary responsibility of the vertex shader is to transform the vertices of the model from object space into clip space and the primary responsibility of the fragment shader is to determine the final color of the screen fragment.

Lets first look at the vertex shader.

### VERTEX SHADER

The vertex shader is the first stage of the rendering pipeline that occurs after the primitive processing stage. The input to the vertex shader is the vertex attributes (like position and color attributes) and the output from the vertex shader is the vertex position in clip-space. Optionally, the vertex shader can produce a number of other outputs (in our example, it will also output a 4-component vector which represents the vertex color).

The vertex attributes that are sent to the vertex shader without modification. If the position of the vertices that are supplied to the vertex shader are expressed in object space, that’s how they will arrive in the vertex shader. All other attributes will also arrive as-is. This means that if we need to transform these vertices into world space, view space, light space, or clip space, then we must do this in the vertex shader. (This can also be done in the Geometry shader if you are using one). The important thing to remember is that the vertex shader must at least output the vertex position in clip-space for it to be processed correctly by the rasterizer stage. Let’s see how we do that.

```
`#version 330 core`
```

The first line in the shader source file specifies which version of GLSL this shader will be compiled with. In our application, we will be creating an OpenGL 3.3 context so it seems logical to also compile our shaders with this version. The version specification is optional but if it is not specified, version “110” is assumed [[1\]](https://www.3dgep.com/introduction-to-opengl-and-glsl/#OpenGL%20Programming%20Guide).

```
`layout(location=0) in vec3 in_position;``layout(location=1) in vec3 in_color;`
```

The next couple lines define the input attributes that are expected to be sent from the application. The first part of the variable definition is called the **layout qualifier**. The layout qualifier determines the attribute index to bind these variables to. In the application, we will bind the vertex position stream to attribute ID **0** and the vertex color stream to attribute ID **1**. The **in** modifier indicates that this variable is input data, that is the value of these variables will be sent from the application. Both of these variables are of type **vec3** which matches the type of the corresponding variables that we defined in the [VertexXYZColor](https://www.3dgep.com/introduction-to-opengl-and-glsl/#VertexXYZColor) struct in the application. It is important to keep in mind that only a single vertex is being sent for every invocation of the vertex shader. That is why the input parameters are not arrays and the vertex shader only needs to define the operations to perform on a single vertex’s attributes. Unlike a CPU program, a GPU program is executed in **parallel**. That is, each vertex in the vertex stream is (conceptually) processed at exactly the same time (in parallel). That is why the vertex shader only needs to operate on a single vertex.

```
`out vec4 v2f_color;`
```

The only explicit output from our vertex shader is a 4-component vector that represents the color of the vertex. The **out** modifier indicates that this is variable will be used as output from this shader. Later we will see that the fragment shader has a corresponding input parameter with the same type and name. The name I chose to use for this parameter starts with **v2f**. This is an arbitrary naming convention that I use to communicate the fact that this variable is sent from the vertex shader to the fragment shader (thus the name **v2f**).

```
`// Model, View, Projection matrix.``uniform mat4 MVP;`
```

The fragment shader also has a single **uniform** variable. The keyword **uniform** indicates that the value of this variable will be the same for each and every invocation of the shader. Unlike the vertex attributes which have been specified as **in** parameters, **uniform** variables are not associated with a stream of input data. The value of the uniform variable is specified once by the application and it does not change while a particular object is being rendered (however the uniform variable can be modified before each draw call). The shader program cannot modify **uniform** variables. Changing a uniform variable in a shader is possible (the compiler may not complain if you do change the contents of a uniform variable) but the changes made to a uniform variable will not be seen by other invocations of the vertex shader. That is, changes to uniform variables are not seen outside the scope of the shader invocation.

```
`void main()``{``    ``gl_Position = MVP * vec4(in_position, 1);``    ``// Just pass the color through directly.``    ``v2f_color = vec4(in_color, 1);``}`
```

And this is the main entry-point for our vertex shader. Every GLSL shader must define at least a **main()** function. This function is what is invoked to process the vertex attributes. In this case, our vertex shader simply transforms the incoming vertex position from object-space into clip-space by pre-multiplying by the model-view-projection matrix. The resulting clip-space position is assigned to the special OpenGL built-in output variable **gl_Position**. At the very least, the vertex shader must assign the clip-space vertex position to the **gl_Position** variable. This value is required by the rasterizer to determine the fragment positions in screen space.

Since the vertex position attribute is passed to the shader as a 3-component vector, we must convert the vertex position to a 4-component vector before we can multiply it by the **MVP** matrix. To do this, we must assign a **1** to the **w-component** of the vector if it represents a point so that translation is taken into consideration. If the attribute represents a direction vector (such as a normal, tangent, or bi-normal ) then we must assign a **0** to the **w-component** so that the direction vector will not be translated by the MVP matrix (since direction vectors should only be rotated into the correct space, but never translated).

The color attribute is simply passed-through to the fragment shader as-is. Since the fragment shader expects a 4-component vector for the color value, we append a **1** to the vector for the **alpha** value. However since we won’t be enabling alpha tests or blending for this application, the 4th component of the color value is completely arbitrary since it won’t be used anyways.

Now that we’ve defined the vertex shader, let’s see how we can define the corresponding fragment shader.

### FRAGMENT SHADER

The fragment shader accepts the explicitly declared output variables from the vertex shader. The **gl_Position** built-in variable that we used in the vertex shader has no meaning in the fragment shader. If we want to use the clip-space position of the vertex in the fragment shader, we must copy it to an explicit output variable in the vertex shader. For our purposes, we don’t need the vertex position in the fragment shader but we do need the color variable from the vertex shader. The primary responsibility of the fragment shader is to output the final color of the current screen fragment. Optionally, the fragment shader can also compute the fragment depth but this is not absolutely necessary because the fragment depth will be determined by the clip-space position that was computed in the vertex shader.

```
`#version 330 core`
```

Similar to the vertex shader, the fragment shader must also define which version of GLSL it should be compiled against.

```
`// Input color from the vertex program.``in vec4 v2f_color;`
```

The only input parameter to the fragment shader is the color attribute that has been passed from the vertex shader. In order for the output variable defined in the vertex shader to be automatically bound to the input variable in the fragment shader, it’s name and type must be the same in both shaders.

Since there are less vertices than screen pixels, we need a way to interpolate the color value between the vertices in order to achieve a smooth transition of the color between vertices in the final fragment. Since interpolating values between vertices is so common, this is actually the default behaviour. In case we don’t want the values between the vertices to be interpolated, we can specify the **flat** variable modifier on both the output variable from the vertex shader and the corresponding input variable to the fragment shader. If we specify the **flat** modifier, the last vertex in the primitive will be used to determine the value for each fragment of the primitive. This is useful for specifying material or texture IDs for the primitive which should not be interpolated across the face of the primitive.

```
`layout (location=0) out vec4 out_color;`
```

The only output variable from the fragment shader is the 4-component color value. Similar to the input variables in the vertex shader, we can use the **layout qualifier** to specify the location that this output variable is bound. In this case, we bind the color output variable to the first attribute location. In OpenGL, it is possible to have multiple render targets (MRT) and each render target will be bound to a specific output attribute. In our simple example we only have a single render target (the color buffer) so specifying the **layout qualifier** here is totally optional.

```
`void main()``{``    ``out_color = v2f_color;``}`
```

Similar to the vertex program, the fragment program must specify an entry-point function called **main()**. The only thing this function does is copy the input variable to the output variable. This is pretty much as easy as it gets but it is possible to do much more in the fragment shader (such as performing multi-texture blending).

Now that we’ve defined the vertex and fragment shader, let’s go back and fill in the functions of the application.

## Initialize OpenGL

Before we define the main entry point for our application, we’ll define a few functions that will be used by main. The first of these functions is the **InitGL** function that will be used to initialize the OpenGL context and create a render window. Callback functions for the windowing events will also be established.

```
`/**`` ``* Initialize the OpenGL context and create a render window.`` ``*/``void InitGL( ``int` `argc, ``char``* argv[] )``{``    ``std::cout << ``"Initialize OpenGL..."` `<< std::endl;` `    ``glutInit( &argc, argv );` `    ``glutSetOption( GLUT_ACTION_ON_WINDOW_CLOSE, GLUT_ACTION_GLUTMAINLOOP_RETURNS );` `    ``int` `iScreenWidth = glutGet(GLUT_SCREEN_WIDTH);``    ``int` `iScreenHeight = glutGet(GLUT_SCREEN_HEIGHT);` `    ``glutInitDisplayMode(GLUT_RGBA|GLUT_ALPHA|GLUT_DOUBLE|GLUT_DEPTH);` `    ``// Create an OpenGL 3.3 core forward compatible context.``    ``glutInitContextVersion( 3, 3 );``    ``glutInitContextProfile(GLUT_CORE_PROFILE);``    ``glutInitContextFlags( GLUT_FORWARD_COMPATIBLE );` `    ``glutInitWindowPosition( ( iScreenWidth - g_iWindowWidth ) / 2, (iScreenHeight - g_iWindowHeight) / 2 );``    ``glutInitWindowSize( g_iWindowWidth, g_iWindowHeight );` `    ``g_iWindowHandle = glutCreateWindow(``"OpenGL Template"``);` `    ``// Register GLUT callbacks.``    ``glutIdleFunc(IdleGL);``    ``glutDisplayFunc(DisplayGL);``    ``glutKeyboardFunc(KeyboardGL);``    ``glutKeyboardUpFunc(KeyboardUpGL);``    ``glutSpecialFunc(SpecialGL);``    ``glutSpecialUpFunc(SpecialUpGL);``    ``glutMouseFunc(MouseGL);``    ``glutMotionFunc(MotionGL);``    ``glutReshapeFunc(ReshapeGL);` `    ``glClearColor(0.0f, 0.0f, 0.0f, 1.0f );``    ``glClearDepth(1.0f);``    ``glEnable(GL_DEPTH_TEST);``    ``glEnable(GL_CULL_FACE);` `    ``std::cout << ``"Initialize OpenGL Success!"` `<< std::endl;``}`
```

The initialization of the OpenGL context and the creation of the render window is the responsibility of FreeGLUT. If we were ambitious, we could do this ourselves by writing a lot of Windows code to create a window and handle the window event loop but for simplicity and to keep the source code primarily platform agnostic, I will just let FreeGLUT to perform all the heavy lifting.

The **glutInit** function allows you to initialize a few of the window properties by specifying them on the command line. For more information see [http://www.opengl.org/documentation/specs/glut/spec3/node10.html](https://www.opengl.org/documentation/specs/glut/spec3/node10.html).

On line 80 the **glutSetOption** function allows us to specify additional options to control the glut main loop. In this case, we want to specify that the main thread returns to the point where the **glutMainLoop()** function was called. This is useful if we need to release any resources that were allocated by our application. For more information see <http://freeglut.sourceforge.net/docs/api.php#StateSetting>.

On lines 82-83, we’ll query the size of the screen so that we can center the render window in the middle of the screen.

The **glutInitDisplayMode** function is used to determine how the render window is created.

- **GLUT_RGBA**: Specify to use an RGB color mode (as opposed to color index mode). Specifying this value does not automatically mean that the color buffer will be initialized with an alpha channel (which is required for alpha blending).
- **GLUT_ALPHA**: Specify to create a color buffer with an alpha component. This is required for alpha blending.
- **GLUT_DOUBLE**: Specify that OpenGL should use double-buffering. This does not mean that the color values are stored in double-precision floating point values but instead 2 framebuffers are created. One which is the currently drawn-to surface and the other which is the currently displayed buffer. Using double buffering helps to reduce artifacts such as screen tearing and flickering.
- **GLUT_DEPTH**: Specify that the framebuffer should be created with a depth buffer. Without this then we could not perform depth tests and primitives may appear in the wrong order when drawn on screen.

The next three lines setup the OpenGL context creation. In this case, we want to create a context that supports the OpenGL 3.3 core specification and we don’t need access to any of the deprecated features prior to OpenGL 3.3. Since we are very progressive and forward thinking we don’t look back on outdated technology ![😉](https://s.w.org/images/core/emoji/13.0.0/svg/1f609.svg) . The **GLUT_FORWARD_COMPATIBLE** flag indicates that it will be forward compatible with newer versions of OpenGL as they become available but not backward compatible (it will not run on devices that do not support OpenGL 3.3).

On lines 92-93 is where the window size and position are initialized and the actual render window is created on line 95. It is important to know that the actual OpenGL context is not created until a valid window handle is returned from the **glutCreateWindow** function. That means that you cannot initialize your OpenGL extensions until a valid render window is created. You should always initialize GLUT before GLEW.

Window callback functions to handle the various window events are registered on lines 97-106. I will describe these functions in more detail later.

On lines 108-111 a few basic OpenGL states are initialized. The **glClearColor** function specifies the color that the color buffer is initialized to when the **glClear** function is called with the **GL_COLOR_BUFFER_BIT** specified. Similarly the **glClearDepth** function specifies the value to initialize the depth buffer to when the **glClear** function is called with the **GL_DEPTH_BUFFER_BIT** is specified. Here **1.0** indicates “the far clipping plane”. The near and far clipping planes are defined in the projection matrix which we will see later.

Since we want to make sure that primitives that are farther away from the viewer are not drawn on top of primitives that are closer to the viewer, we must enable depth testing. The depth test stage of the rendering pipeline will discard fragments if there is already something drawn closer to the viewer.

Enabling cull face is more of a rendering optimization. Enabling cull face allows the primitive assembly stage to reject entire primitives if the primitive is not facing the viewer. By default, triangles whose vertices are specified in counter-clockwise order are considered “front-facing” but this can be changed using the **glFrontFace** function.

After we have initialized OpenGL and created a render window, we can setup the extensions using GLEW.

## Initialize GLEW

If we want to use any part of the OpenGL specification higher than OpenGL 1.1 (which we do) then we need to initialize all of the extensions that we will require. Luckily we don’t have to go through the pain of initializing all of the required OpenGL extensions that we will be using. For more information regarding OpenGL extensions, you can refer to one of my previous articles titled [OpenGL extensions](https://www.3dgep.com/?p=2415).

```
`void InitGLEW()``{``    ``glewExperimental = GL_TRUE;``    ``if` `( glewInit() != GLEW_OK )``    ``{``        ``std::cerr << ``"There was a problem initializing GLEW. Exiting..."` `<< std::endl;``        ``exit``(-1);``    ``}` `    ``// Check for 3.3 support.``    ``// I've specified that a 3.3 forward-compatible context should be created.``    ``// so this parameter check should always pass if our context creation passed.``    ``// If we need access to deprecated features of OpenGL, we should check``    ``// the state of the GL_ARB_compatibility extension.``    ``if` `( !GLEW_VERSION_3_3 )``    ``{``        ``std::cerr << ``"OpenGL 3.3 required version support not present."` `<< std::endl;``        ``exit``(-1);``    ``}` `#ifdef _WIN32``    ``if` `( WGLEW_EXT_swap_control )``    ``{``        ``wglSwapIntervalEXT(0); ``// Disable vertical sync``    ``}``#endif``}`
```

This relatively simple function simply initializes the GLEW framework. The **glewInit** function must only be called after the OpenGL context has been created (if GLUT was able to create a render window). Calling this function will initialize all of the extensions that have been introduced since OpenGL 1.1. Initializing GLEW may fail if no OpenGL context has been created yet.

Since the machine that compiles the application will be different than the machine running the application, we need to check at runtime if the OpenGL 3.3 core specification is supported. The version of OpenGL that is supported is dependent on the graphics card and the graphics drivers in the user’s computer so it’s not possible to build an OpenGL application that will run on computers that don’t have support for the required specification. The OpenGL 3.3 specification was released in 2010 [[9\]](https://www.3dgep.com/introduction-to-opengl-and-glsl/#History%20of%20OpenGL) so any decent computer that was purchased in the last few years will support OpenGL 3.3. Checking for OpenGL 3.3 core support is redundant here because we asked GLUT to create an OpenGL 3.3 context. If that succeeded then the check for OpenGL 3.3 core functionality must also pass.

On lines 136-139 we check for the **WGLEW_EXT_swap_control** extension support. If it is found, then we can use the **wglSwapIntervalEXT** function to disable v-sync so we can get a ridiculous frame-rate (on my PC I’m getting 6,500 FPS). It is important to note that this extension is only available on Windows (thus the W prefix) and therefore we should wrap it’s usage with the **_WIN32** macro guard to remain platform agnostic.

Now that we have defined the basic OpenGL preamble, let’s write something interesting.

## Load a Shader

Now that we have created the OpenGL context and initialized and verified extension support, let’s load the shaders which we have written earlier. All shaders in OpenGL are loaded in the same way so we can write a generic function to load any shader in OpenGL. We only need to know what type of shader object it is (Vertex, Fragment, Geometry, Tessellation Control, Tessellation Evaluation, or Compute shader) and the path to the shader file we want to load.

Let’s analyze this function in a few steps. The first step is to load the contents of the shader file into a character array. We’ll use the power of STL to accomplish this.

```
`// Loads a shader and returns the compiled shader object.``// If the shader source file could not be opened or compiling the ``// shader fails, then this function returns 0.``GLuint LoadShader( GLenum shaderType, ``const` `std::string& shaderFile )``{``    ``std::ifstream ifs;` `    ``// Load the shader source file.``    ``ifs.open(shaderFile);` `    ``if` `( !ifs )``    ``{``        ``std::cerr << ``"Can not open shader file: \""` `<< shaderFile << ``"\""` `<< std::endl;``        ``return` `0;``    ``}` `    ``std::string source( std::istreambuf_iterator<``char``>(ifs), (std::istreambuf_iterator<``char``>()) );``    ``ifs.close();`
```

These first few lines simply opens the shader file and checks to see if the file was successfully opened. If it was opened, the contents of the file are copied into the **source** string.

The next step is to create a new shader object, copy the source code into the shader object and compile it.

```
`// Create a shader object.``GLuint shader = glCreateShader( shaderType );` `// Load the shader source for each shader object.``const` `GLchar* sources[] = { source.c_str() };``glShaderSource( shader, 1, sources, NULL );``glCompileShader( shader );`
```

If we did everything correctly, then we’d be finished loading the shader program but how many programmers can claim they have never generated a compiler error? Not many I think so the next step will be to check for compiler errors.

```
`    ``// Check for errors``    ``GLint compileStatus;``    ``glGetShaderiv( shader, GL_COMPILE_STATUS, &compileStatus ); ``    ``if` `( compileStatus != GL_TRUE )``    ``{``        ``GLint logLength;``        ``glGetShaderiv( shader, GL_INFO_LOG_LENGTH, &logLength );``        ``GLchar* infoLog = ``new` `GLchar[logLength];``        ``glGetShaderInfoLog( shader, logLength, NULL, infoLog );` `#ifdef _WIN32``        ``OutputDebugString(infoLog);``#else``        ``std::cerr << infoLog << std::endl;``#endif``        ``delete` `infoLog;``        ``return` `0;``    ``}`
```

On line 175, we use the **glGetShaderiv** function with the **GL_COMPILE_STATUS** parameter to check the result of compiling the shader. If everything was okay, then the compiler status should contain the value **GL_TRUE** otherwise an error occurred while compiling our shader.

If an error occurred, we can use the function **glGetShaderiv** again, this time with the parameter **GL_INFO_LOG_LENGTH** to check the size of the log file. Then create an array of that length to store the result of the shader log using the function **glGetShaderInfoLog**.

If you are using Visual Studio, the **OutputDebugString** method can be used to output the error log directly to the Visual Studio output log. Otherwise you can use the **std::cerr** to output the error log directly to the default error stream.

Don’t forget to delete the **infoLog** array and return 0 to indicate the shader failed to load.

If there wasn’t any compiler errors, the shader object ID is returned.

```
`    ``return` `shader;``}`
```

## Create a Shader Program

Using the **LoadShader** function previously defined, we can load the vertex and fragment programs that we created. Now we need to link the different shader objects together to create a shader program. Before we can link the shader program, we need to attach all of the shader objects to the shader program. Similar to compiling, there might be some problems linking the shader program. If so, we can print any linker errors that may have occurred.

```
`// Create a shader program from a set of compiled shader objects.``GLuint CreateShaderProgram( std::vector<GLuint> shaders )``{``    ``// Create a shader program.``    ``GLuint program = glCreateProgram();` `    ``// Attach the appropriate shader objects.``    ``for``( GLuint shader: shaders )``    ``{``        ``glAttachShader( program, shader );``    ``}`
```

The **CreateShaderProgram** function takes an STL vector which contains a list of all the shaders that should be linked together into a single shader program.

First we need to create a new shader program with the **glCreateProgram** function. Then we need to attach each shader using the **glAttachShader** function. (Do you like the range-based for loop I’m using here? One of the nice features of C++11 that was added in Visual Studio 2012. This won’t compile in Visual Studio 2010 or earlier. If you are using a compiler that does not have full support for C++11, you may have to convert this line into the older iterator method).

Next, we want to link the program and check for errors.

```
`    ``// Link the program``    ``glLinkProgram(program);` `    ``// Check the link status.``    ``GLint linkStatus;``    ``glGetProgramiv(program, GL_LINK_STATUS, &linkStatus );``    ``if` `( linkStatus != GL_TRUE )``    ``{``        ``GLint logLength;``        ``glGetProgramiv( program, GL_INFO_LOG_LENGTH, &logLength );``        ``GLchar* infoLog = ``new` `GLchar[logLength];` `        ``glGetProgramInfoLog( program, logLength, NULL, infoLog );` `#ifdef _WIN32``        ``OutputDebugString(infoLog);``#else``        ``std::cerr << infoLog << std::endl;``#endif` `        ``delete` `infoLog;``        ``return` `0;``    ``}`
```

To link the shader program, we use the **glLinkProgram** function. We can check the link status using the function **glGetProgramiv** and passing **GL_LINK_STATUS** as the parameter. If the link status is not **GL_TRUE** then something went wrong and we can get the program info log with the **glGetProgramInfoLog** function.

If the shader program linked okay, then we simply return the shader program ID.

```
`    ``return` `program;``}`
```

Now let’s put these pieces together and create our main entry point for our OpenGL application.

## Main

No program is complete without the **main** entry point. First, we’ll initialize some variables that will be used later.

```
`int` `main( ``int` `argc, ``char``* argv[] )``{``    ``g_PreviousTicks = std::``clock``();``    ``g_A = g_W = g_S = g_D = g_Q = g_E = 0;` `    ``g_InitialCameraPosition = glm::vec3( 0, 0, 10 );``    ``g_Camera.SetPosition( g_InitialCameraPosition );``    ``g_Camera.SetRotation( g_InitialCameraRotation );`
```

On line 236, we’ll initialize a clock parameter with the current system clock time. This is a quick and dirty low-resolution timer which works well enough for our purposes. If you need a higher resolution timer, you may want to look at the new Chrono library that was added in the C++11 standard [[http://www.cplusplus.com/reference/chrono/\]](http://www.cplusplus.com/reference/chrono/).

The **g_A**, **g_W**, **g_S**, **g_D**, **w_Q**, and **g_E** integer variables will be set to 1 if the corresponding key on the keyboard is pressed, and 0 otherwise. We can use these variables to pan the camera around.

The camera will initially be setup 10 units in the positive Z axis. In OpenGL (using a right-handed coordinate system), a non-transformed (identity matrix) camera is looking straight down the negative Z axis so moving it in the positive Z axis will have our camera looking back at the origin (where our untransformed cube will be located). The camera will initially have no rotation applied to it.

Next, let’s initialize GLUT and GLEW.

```
`InitGL(argc, argv);``InitGLEW();`
```

Here we just invoke the initialization functions that we defined earlier.

Now let’s load our shaders and create our shader program.

```
`// Load some shaders.``GLuint vertexShader = LoadShader( GL_VERTEX_SHADER, ``"../data/shaders/simpleShader.vert"` `);``GLuint fragmentShader = LoadShader( GL_FRAGMENT_SHADER, ``"../data/shaders/simpleShader.frag"` `);` `std::vector<GLuint> shaders;``shaders.push_back(vertexShader);``shaders.push_back(fragmentShader);` `// Create the shader program.``g_ShaderProgram = CreateShaderProgram( shaders );``assert``( g_ShaderProgram != 0 );`
```

We’ll use the **LoadShader** function that we defined earlier to load the two shader objects that we need. If you saved your shader files in a different location than me, you will need to update the file locations.

Then we create vector and push the shader objects into it to be passed to the **CreateShaderProgram** function we defined earlier.

If everything was okay then we should have a valid shader program as a result.

Now we need to find out how to connect our vertex attributes to the shader program. In this case, we already know the attribute locations for our two vertex attribute streams (vertex position and vertex color) because we explicitly specified them using the layout qualifiers but if we change the shader program then the attribute locations may change. It is also possible that the linker optimizes unused attributes and uniform variables away if they are not being used anywhere in the final program so it is always a good idea to explicitly request the attribute locations after the shader program has been linked.

```
`GLint positionAtribID = glGetAttribLocation( g_ShaderProgram, ``"in_position"` `);``GLint colorAtribID = glGetAttribLocation( g_ShaderProgram, ``"in_color"` `);``g_uniformMVP = glGetUniformLocation( g_ShaderProgram, ``"MVP"` `);`
```

The **glGetAttribLocation** function can be used to query the location ID for a named vertex attributes (variables declared in the shader program with the **in** modifier).

The **glGetUniformLocation** function can be used to query the location ID for a named uniform variables (variables declared in the shader program with the **uniform** modifier).

Now we’ll load the vertex data in the GPU. We’ll use a Vertex Array Object (VAO) to encapsulate all of the vertex attributes that define our geometry.

```
`// Create a VAO for the cube.``glGenVertexArrays( 1, &g_vaoCube );``glBindVertexArray( g_vaoCube );`
```

First we create a new VAO and bind it to make it active.

Next, we’ll create two Vertex Buffer Objects (VBO), one for the vertex data, and another for the index data.

```
`GLuint vertexBuffer, indexBuffer;``glGenBuffers( 1, &vertexBuffer );``glGenBuffers( 1, &indexBuffer );`
```

Then we need to bind the buffers and populate them with the vertex data.

```
`glBindBuffer( GL_ARRAY_BUFFER, vertexBuffer );``glBufferData( GL_ARRAY_BUFFER, ``sizeof``(g_Vertices), g_Vertices, GL_STATIC_DRAW );` `glBindBuffer( GL_ELEMENT_ARRAY_BUFFER, indexBuffer );``glBufferData( GL_ELEMENT_ARRAY_BUFFER, ``sizeof``(g_Indices), g_Indices, GL_STATIC_DRAW );`
```

For more information on using VBOs, you can refer to my previous article titled [[Using OpenGL Vertex Buffer Objects\]](https://www.3dgep.com/?p=2596).

After we have copied the model data to the VBOs, we need to specify which attributes are mapped to which parts of the VBO. We must specify that the position data in the VBO stream maps to attribute location 0 and the color data in the VBO stream maps to attribute location 1.

```
`glVertexAttribPointer( positionAtribID, 3, GL_FLOAT, ``false``, ``sizeof``(VertexXYZColor), MEMBER_OFFSET(VertexXYZColor,m_Pos) );``glEnableVertexAttribArray( positionAtribID );` `glVertexAttribPointer( colorAtribID, 3, GL_FLOAT, ``false``, ``sizeof``(VertexXYZColor), MEMBER_OFFSET(VertexXYZColor,m_Color) );``glEnableVertexAttribArray( colorAtribID );`
```

The **glVertexAttribPointer** function allows us to map arbitrary vertex attributes to arbitrary attribute locations. This method has the following signature:

```
`void glVertexAttribPointer( GLuint index, GLint size, GLenum type, GLboolean normalized, GLsizei stride, ``const` `GLvoid * pointer);`
```

And takes the following arguments [[14\]](https://www.3dgep.com/introduction-to-opengl-and-glsl/#glVertexAttribPointer):

- **GLuint index**: Specifies the index of the generic vertex attribute to be modified.
- **GLint size**: Specifies the number of components per generic vertex attribute. Must be 1, 2, 3, 4.
- **GLenum type**: Specifies the data type of each component in the array. The symbolic constants **GL_BYTE**, **GL_UNSIGNED_BYTE**, **GL_SHORT**, **GL_UNSIGNED_SHORT**, **GL_INT**, **GL_UNSIGNED_INT**, **GL_HALF_FLOAT**, **GL_FLOAT**, **GL_DOUBLE**, **GL_INT_2_10_10_10_REV**, and **GL_UNSIGNED_INT_2_10_10_10_REV** are accepted.
- **GLboolean normalized**: Specifies whether fixed-point data values should be normalized (**GL_TRUE**) or converted directly as fixed-point values (**GL_FALSE**) when they are accessed.
- **GLsizei stride**: Specifies the byte offset between consecutive generic vertex attributes. If stride is 0, the generic vertex attributes are understood to be tightly packed in the array.
- **const GLvoid \* pointer**: Specifies an offset of the first component of the first generic vertex attribute in the array in the data store of the buffer currently bound to the GL_ARRAY_BUFFER target.

In order for the vertex attributes to be activated in the VAO, we must enable them using the **glEnableVertexAttribArray** function passing the generic vertex attribute location as the only parameter.

Now that we have specified and activated all of the vertex attributes that describe our geometry, we can unbind and disable any states that we have activated during the initialization.

```
`// Make sure we disable and unbind everything to prevent rendering issues later.``glBindVertexArray( 0 );``glBindBuffer( GL_ARRAY_BUFFER, 0 );``glBindBuffer( GL_ELEMENT_ARRAY_BUFFER, 0 );``glDisableVertexAttribArray( positionAtribID );``glDisableVertexAttribArray( colorAtribID );`
```

We unbind the currently active VAO and VBO’s by activating the default VAO and VBO **0**. This is equivalent to “disabling” the VAO and VBO’s.

We should also disable the generic vertex attributes that were enabled while setting up the VAO.

And now we can kick-off the game loop!

```
`    ``glutMainLoop();``}`
```

## Reshape

Whenever the render window is resized (or sized for the first time when it is created before the first draw call is invoked) the **ReshapeGL** callback function will be invoked. We can use this function to setup our camera parameters correctly based on the size of the render window.

```
`void ReshapeGL( ``int` `w, ``int` `h )``{``    ``if` `( h == 0 )``    ``{``        ``h = 1;``    ``}` `    ``g_iWindowWidth = w;``    ``g_iWindowHeight = h;` `    ``g_Camera.SetViewport( 0, 0, w, h );``    ``g_Camera.SetProjectionRH( 60.0f, w/(``float``)h, 0.1f, 100.0f );` `    ``glutPostRedisplay();``}`
```

The first few lines we want to make sure that someone doesn’t try to crash our program by making the render window too small. Not checking for this could result in a divide by zero error so let’s not let that happen!

On line 302, we setup the camera’s viewport to fill the entire render window.

On line 303 the camera’s projection matrix is initialized with a 60 degree field-of-view, an aspect ratio that matches the aspect ratio of the window, a near clipping plane of 0.1 units and a far clipping plane of 100 units. Remind me to make an article about projection matrices so that I can link to it here if none of this makes sense to you.

The **glutPostRedisplay** method tells GLUT to add a PAINT event to the current window’s event queue. This will cause the scene to get redrawn.

## Display

The **DisplayGL** callback function will be invoked whenever the render window needs to be redrawn (which is guaranteed to occur whenever the **glutPostRedisplay** method is called).

```
`void DisplayGL()``{``    ``glClear(GL_COLOR_BUFFER_BIT|GL_DEPTH_BUFFER_BIT);` `    ``glBindVertexArray( g_vaoCube );``    ``glUseProgram( g_ShaderProgram );` `    ``glm::mat4 mvp = g_Camera.GetProjectionMatrix() * g_Camera.GetViewMatrix() * glm::toMat4(g_Rotation);``    ``glUniformMatrix4fv( g_uniformMVP, 1, GL_FALSE, glm::value_ptr(mvp) );` `    ``glDrawElements( GL_TRIANGLES, 36, GL_UNSIGNED_INT, BUFFER_OFFSET(0) );` `    ``glUseProgram(0);``    ``glBindVertexArray(0);` `    ``glutSwapBuffers();``}`
```

First, we want to make sure we are working with a new canvas to paint on. To do that we use the **glClear** method passing the **GL_COLOR_BUFFER_BIT** flag to clear the color buffer and the **GL_DEPTH_BUFFER_BIT** flag to clear the depth buffer. These buffers will be cleared to the values specified by the **glClearColor** and the **glClearDepth** functions that was shown in the **InitGL** function.

On line 312, we bind the VAO and activate the shader program that we setup in the main function.

If the camera moves or the cube rotates, we need to update the MVP matrix. We do this by first computing the combined MVP matrix and assigning this MVP matrix to the uniform variable in the shader using the **glUniformMatrix4fv** method. This method takes the following parameters [[15\]](https://www.3dgep.com/introduction-to-opengl-and-glsl/#glUniform):

- **GLint location**: Specifies the location of the uniform value to be modified.
- **GLsizei count**: Specifies the number of matrices that are to be modified. This should be 1 if the targeted uniform variable is not an array of matrices, and 1 or more if it is an array of matrices.
- **GLboolean transpose**: Specifies whether to transpose the matrix as the values are loaded into the uniform variable.
- **const GLfloat \* value**: Specifies a pointer to an array of count values that will be used to update the specified uniform variable.

The **glDrawElements** method is used to kick off all of those vertices down the rendering pipeline.

We should not forget to unbind the VAO and the shader program so that we leave the OpenGL context in the state we found it.

The **glutSwapBuffers** will cause the back buffer (the buffer we just rendered our geometry to) to become the front buffer and be displayed on the screen (remember we’re using double buffering).

Now we’ve seen the draw function. What about the update function? In the next section I’ll describe the Idle function which is used to update the game logic.

## Idle

The **IdleGL** function will run whenever no other events need to be processed on the window’s event queue. Since we don’t really know how much time will pass between calls to the Idle function (this may be different on different computers) we should try to determine the elapsed time each frame and use that to move things smoothly in the scene.

For this implementation, we are only updating the position of the camera when the user presses the W, A, S, D, Q, and E keys on the keyboard. If so, we will pan the camera at 1 unit/second and if the Shift key is held down, we will move the camera at 5 units/second.

```
`void IdleGL()``{``    ``g_CurrentTicks = std::``clock``();``    ``float` `deltaTicks = (``float``)( g_CurrentTicks - g_PreviousTicks );``    ``g_PreviousTicks = g_CurrentTicks;` `    ``float` `fDeltaTime = deltaTicks / (``float``)CLOCKS_PER_SEC;` `    ``float` `cameraSpeed = 1.0f;``    ``if` `( g_bShift )``    ``{``        ``cameraSpeed = 5.0f;``    ``}` `    ``g_Camera.Translate( glm::vec3( g_D - g_A, g_Q - g_E, g_S - g_W ) * cameraSpeed * fDeltaTime );` `    ``glutPostRedisplay();``}`
```

In the first few lines, we’ll compute the elapsed time since the Idle function was previously called.

If the Shift key is pressed, the camera will pan at 5 units/second. If not, the camera will pan 1 unit / second.

On line 340, the camera is translated based on the keys that are pressed. The A, D keys are used to move the camera in the X axis (left, right), the Q, E keys are used to move the camera in the Y axis (up, down) and the W, S keys are used to move the camera in the Z axis (forward, backward).

The **glutPostRedisplay** function will ensure the screen is redrawn.

The next functions that I’ll describe are the keyboard and mouse handlers.

## Key Down

The **KeyboardGL** function will handle events when a key is pressed on the keyboard.

```
`void KeyboardGL( unsigned ``char` `c, ``int` `x, ``int` `y )``{``    ``switch` `( c )``    ``{``    ``case` `'w'``:``    ``case` `'W'``:``        ``g_W = 1;``        ``break``;``    ``case` `'a'``:``    ``case` `'A'``:``        ``g_A = 1;``        ``break``;``    ``case` `'s'``:``    ``case` `'S'``:``        ``g_S = 1;``        ``break``;``    ``case` `'d'``:``    ``case` `'D'``:``        ``g_D = 1;``        ``break``;``    ``case` `'q'``:``    ``case` `'Q'``:``        ``g_Q = 1;``        ``break``;``    ``case` `'e'``:``    ``case` `'E'``:``        ``g_E = 1;``        ``break``;``    ``case` `'r'``:``    ``case` `'R'``:``        ``g_Camera.SetPosition( g_InitialCameraPosition );``        ``g_Camera.SetRotation( g_InitialCameraRotation );``        ``g_Rotation = glm::quat();``        ``break``;``    ``case` `27:``        ``glutLeaveMainLoop();``        ``break``;``    ``}``}`
```

There is nothing surprising happening here. If any of the W, A, S, D, Q, or E keys are pressed on the keyboard, the corresponding flag is activated which controls the camera panning. If the R key is pressed, the camera and objects transformations are reset to their default values and if the escape key (27) is pressed, the glut main loop is terminated.

## Key Up

The **KeyboardUpGL** function is called when a key is released on the keyboard. In this case, we’ll just reset the state of the appropriate flag when a key is released.

```
`void KeyboardUpGL( unsigned ``char` `c, ``int` `x, ``int` `y )``{``    ``switch` `( c )``    ``{``    ``case` `'w'``:``    ``case` `'W'``:``        ``g_W = 0;``        ``break``;``    ``case` `'a'``:``    ``case` `'A'``:``        ``g_A = 0;``        ``break``;``    ``case` `'s'``:``    ``case` `'S'``:``        ``g_S = 0;``        ``break``;``    ``case` `'d'``:``    ``case` `'D'``:``        ``g_D = 0;``        ``break``;``    ``case` `'q'``:``    ``case` `'Q'``:``        ``g_Q = 0;``        ``break``;``    ``case` `'e'``:``    ``case` `'E'``:``        ``g_E = 0;``        ``break``;` `    ``default``:``        ``break``;``    ``}``}`
```

Next, we’ll handle the special keys (non-printable characters).

## Special Keys

```
`void SpecialGL( ``int` `key, ``int` `x, ``int` `y )``{``    ``switch``( key )``    ``{``    ``case` `GLUT_KEY_SHIFT_L:``    ``case` `GLUT_KEY_SHIFT_R:``        ``{``            ``g_bShift = ``true``;``        ``}``        ``break``;``    ``}``}` `void SpecialUpGL( ``int` `key, ``int` `x, ``int` `y )``{``    ``switch``( key )``    ``{``    ``case` `GLUT_KEY_SHIFT_L:``    ``case` `GLUT_KEY_SHIFT_R:``        ``{``            ``g_bShift = ``false``;``        ``}``        ``break``;``    ``}``}`
```

The special keys are sent as integer (32-bit) values. Regular keyboard keys are sent as char (8-bit) values. That’s why we need different functions to handle different keys on the keyboard. Annoying.. I know!

## Mouse Handling

The **MouseGL** function gets called when the user presses the mouse button on the screen. We’ll use this function to initialize the previous mouse position so we can calculate a delta when the mouse is dragged (handled in another function).

```
`void MouseGL( ``int` `button, ``int` `state, ``int` `x, ``int` `y )``{``    ``g_MousePos = glm::ivec2(x,y);``}`
```

When the mouse is dragged, the **MotionGL** function is called.

```
`void MotionGL( ``int` `x, ``int` `y )``{``    ``glm::ivec2 mousePos = glm::ivec2( x, y );``    ``glm::vec2 delta = glm::vec2( mousePos - g_MousePos );``    ``g_MousePos = mousePos;` `    ``std::cout << ``"dX: "` `<< delta.x << ``" dy: "` `<< delta.y << std::endl;` `    ``glm::quat rotX = glm::angleAxis<``float``>( glm::radians(delta.y) * 0.5f, glm::vec3(1, 0, 0) );``    ``glm::quat rotY = glm::angleAxis<``float``>( glm::radians(delta.x) * 0.5f, glm::vec3(0, 1, 0) );` `    ``//g_Camera.Rotate( rotX * rotY );``    ``g_Rotation = ( rotX * rotY ) * g_Rotation;``}`
```

The first few lines of this function compute the distance the mouse has moved since either the mouse was first pressed or since the last time **MotionGL** was called. Based on the distance the mouse moved, we compute 2 rotation quaternions. One that expresses a rotation about the Y-axis (yaw) and another that expresses a rotation about the X-axis (pitch). Combining these two quaternions with the current rotation quaternion produces the final rotation. Now we can use the mouse to rotate the cube in the scene.

# Conclusion and Final Remarks

When you compile and run the program, you should see something similar to what is shown below. Use your mouse to rotate the cube.



<iframe scrolling="no" src="https://www.3dgep.com/demos/rotating_cube_smooth_shading.html" style="border: 0px; font-family: inherit; font-size: 15px; font-style: inherit; font-weight: inherit; margin: 0px; outline: 0px; padding: 0px; vertical-align: baseline; max-width: 100%; width: 566.266px; min-height: 380px;"></iframe>

Rotating Cube

# Download the Source

You can download the source code for this demo here:

[![img](https://www.3dgep.com/wp-content/uploads/2013/10/zip-icon_03-e1382533375997.png)OpenGL_GLSL.zip](https://drive.google.com/uc?export=download&id=0B0ND0J8HHfaXMGJJanJzam1CMjg)

# References

[1] Shreiner, D., Sellers, G., Kessenich, J. and Licea-Kane, B. 2013. *OpenGL Programming Guide.* 8th ed. Pearson Education, Inc.

[2] Wikipedia. 2014. *OpenGL.* [online] Available at: [http://en.wikipedia.org/wiki/OpenGL](https://en.wikipedia.org/wiki/OpenGL) [Accessed: 13 Jan 2014].

[3] Opengl.org. 2014. *OpenGL 2.1 Reference Pages.* [online] Available at: [http://www.opengl.org/sdk/docs/man2/](https://www.opengl.org/sdk/docs/man2/) [Accessed: 13 Jan 2014].

[4] Sgi.com. 2014. *Merging Textures with Specular Highlights.* [online] Available at: <ftp://ftp.sgi.com/opengl/contrib/blythe/advanced99/notes/node63.html> [Accessed: 13 Jan 2014].

[5] Opengl.org. 2014. *4.1.3 Alpha test.* [online] Available at: [http://www.opengl.org/documentation/specs/version1.1/glspec1.1/node96.html](https://www.opengl.org/documentation/specs/version1.1/glspec1.1/node96.html) [Accessed: 13 Jan 2014].

[6] Opengl.org. 2014. *4.1.5 Depth buffer test.* [online] Available at: [http://www.opengl.org/documentation/specs/version1.1/glspec1.1/node98.html](https://www.opengl.org/documentation/specs/version1.1/glspec1.1/node98.html) [Accessed: 13 Jan 2014].

[7] Opengl.org. 2014. *4.1.4 Stencil test.* [online] Available at: [http://www.opengl.org/documentation/specs/version1.1/glspec1.1/node97.html](https://www.opengl.org/documentation/specs/version1.1/glspec1.1/node97.htmlhttp://www.opengl.org/documentation/specs/version1.1/glspec1.1/node97.html) [Accessed: 13 Jan 2014].

[8] Wikipedia. 2014. *Graphics Interchange Format.* [online] Available at: [http://en.wikipedia.org/wiki/Graphics_Interchange_Format](https://en.wikipedia.org/wiki/Graphics_Interchange_Format) [Accessed: 20 Jan 2014].

[9] Opengl.org. 2014. *History of OpenGL – OpenGL.org.* [online] Available at: [http://www.opengl.org/wiki/History_of_OpenGL](https://www.opengl.org/wiki/History_of_OpenGL) [Accessed: 20 Jan 2014].

[10] Opengl.org. 2014. *Tessellation – OpenGL.org.* [online] Available at: [http://www.opengl.org/wiki/Tessellation](https://www.opengl.org/wiki/Tessellation) [Accessed: 20 Jan 2014].

[11] Freeglut.sourceforge.net. 2014. *The freeglut Project :: About.* [online] Available at: <http://freeglut.sourceforge.net/> [Accessed: 20 Jan 2014].

[12] Glew.sourceforge.net. 2014. GLEW: *The OpenGL Extension Wrangler Library.* [online] Available at: <http://glew.sourceforge.net/> [Accessed: 20 Jan 2014].

[13] Glm.g-truc.net. 2014. *OpenGL Mathematics.* [online] Available at: <http://glm.g-truc.net/0.9.5/index.html> [Accessed: 20 Jan 2014].

[14] Opengl.org. 2014. glVertexAttribPointer – OpenGL 3.3 Reference Pages. [online] Available at: [http://www.opengl.org/sdk/docs/man3/xhtml/glVertexAttribPointer.xml](https://www.opengl.org/sdk/docs/man3/xhtml/glVertexAttribPointer.xml) [Accessed: 11 Feb 2014].

Opengl.org. 2014. glUniform – OpenGL 3.3 Reference Pages. [online] Available at: [http://www.opengl.org/sdk/docs/man3/xhtml/glUniform.xml](https://www.opengl.org/sdk/docs/man3/xhtml/glUniform.xml) [Accessed: 11 Feb 2014].

This entry was posted in [GLSL](https://www.3dgep.com/category/graphics-programming/glsl/), [OpenGL](https://www.3dgep.com/category/graphics-programming/opengl/) and tagged [3D](https://www.3dgep.com/tag/3d/), [Fragment Shader](https://www.3dgep.com/tag/fragment-shader/), [freeGLUT](https://www.3dgep.com/tag/freeglut/), [glew](https://www.3dgep.com/tag/glew/), [glm](https://www.3dgep.com/tag/glm/), [GLSL](https://www.3dgep.com/tag/glsl/), [glut](https://www.3dgep.com/tag/glut/), [OpenGL](https://www.3dgep.com/tag/opengl/), [OpenGL mathmatics library](https://www.3dgep.com/tag/opengl-mathmatics-library/), [rendering](https://www.3dgep.com/tag/rendering/), [Vertex Shader](https://www.3dgep.com/tag/vertex-shader/) by [Jeremiah](https://www.3dgep.com/author/jeremiah/). Bookmark the [permalink](https://www.3dgep.com/introduction-to-opengl-and-glsl/).

## 7 THOUGHTS ON “INTRODUCTION TO OPENGL AND GLSL”

1. ![img](https://secure.gravatar.com/avatar/60d7179114630b20e89c1c97208f3a69?s=68&d=mm&r=g)kozuki on [April 10, 2014 at 2:24 pm](https://www.3dgep.com/introduction-to-opengl-and-glsl/#comment-4792) said:

   Jeremiah,
   thank you for your nice tutorial and site, and
   I really appreciate your willing to share what
   you know.

   [Reply ↓](https://www.3dgep.com/introduction-to-opengl-and-glsl/#comment-4792)

2. ![img](https://secure.gravatar.com/avatar/e759a00faf68d792c5bdb7850d045e02?s=68&d=mm&r=g)Ed Fels on [May 2, 2014 at 7:23 pm](https://www.3dgep.com/introduction-to-opengl-and-glsl/#comment-8378) said:

   This is the best tutorial on OpenGL for beginners like me.You are an excellent teacher.Worked perfectly.I can’t believe I learnt both Vertex and Pixel Shaders at once in one simple tutorial.Thanks a lot.Awaiting more,Especially Global Illumination turorials.Keep up the good work.

   [Reply ↓](https://www.3dgep.com/introduction-to-opengl-and-glsl/#comment-8378)

3. ![img](https://secure.gravatar.com/avatar/afa30a120566806b6204244899877023?s=68&d=mm&r=g)MrEd on [July 1, 2014 at 11:57 pm](https://www.3dgep.com/introduction-to-opengl-and-glsl/#comment-24905) said:

   An excellent tutorial on GLSL with step by step explanation… This is what I expected! thank you!!

   [Reply ↓](https://www.3dgep.com/introduction-to-opengl-and-glsl/#comment-24905)

4. ![img](https://secure.gravatar.com/avatar/7a22de51bc4582a3883bdaba703c0692?s=68&d=mm&r=g)Augus1990 on [November 13, 2014 at 9:11 am](https://www.3dgep.com/introduction-to-opengl-and-glsl/#comment-35590) said:

   (…We should not forget to unbind the VAO and the shader program so that we leave the OpenGL context in the state we found it….)

   Why? bind and unbind the VAO to many times won’t decrease the performance?

   Good tutorial, but do you have an example to render a cube with 2 or more textures please?

   Thank you.

   [Reply ↓](https://www.3dgep.com/introduction-to-opengl-and-glsl/#comment-35590)

   - ![img](https://secure.gravatar.com/avatar/33efe888a2e9f3247cf1cc79f56518c2?s=39&d=mm&r=g)[admin](https://3dgep.com/)on [December 9, 2014 at 10:16 am](https://www.3dgep.com/introduction-to-opengl-and-glsl/#comment-37762) said:

     Augus,

     We must unbind the VAO and shader so that they don’t remain bound in the next draw call.

     There is an article on multitexture terrain which also shows how you can bind multiple textures in a single draw call.

     Regards,

     Jeremiah van Oosten

     [Reply ↓](https://www.3dgep.com/introduction-to-opengl-and-glsl/#comment-37762)

5. ![img](https://secure.gravatar.com/avatar/d18cd773808da82bb66b642e0b93d1bb?s=68&d=mm&r=g)chdig on [April 3, 2016 at 2:17 pm](https://www.3dgep.com/introduction-to-opengl-and-glsl/#comment-51349) said:

   Hi there,

   Thanks for putting this up. I have a slight problem in the setup, however.

   you mention

   “FreeGLUT provides Visual Studio 2012 project files so we can add this project to our solution directly.

   Add an existing project to your solution and select the FreeGLUT Visual Studio 2012 project file located in the “extern\freeglut-2.8.1\VisualStudio\2012” folder.”

   however, I can’t seem to find these project files in the freeglut download I got from the link you specified. How would I get over this?

   thanks again

   [Reply ↓](https://www.3dgep.com/introduction-to-opengl-and-glsl/#comment-51349)

   - ![img](https://secure.gravatar.com/avatar/00f2d6dcb1d4602c73bb571194789ab0?s=39&d=mm&r=g)[Jeremiah van Oosten](https://3dgep.com/)on [July 7, 2016 at 1:17 pm](https://www.3dgep.com/introduction-to-opengl-and-glsl/#comment-52117) said:

     Chdig,

     FreeGLUT 3.0.0 no longer provides the Visual Studio project files. Instead they are using cmake ([https://cmake.org/](https://cmake.org/download/)) and you have to generate the project files for your build toolchain yourself using the cmake tool.

     If you download the zip file at the end of the article, you will see the project files in the “extern\freeglut-2.8.1\VisualStudio\2012\” folder.

     [Reply ↓](https://www.3dgep.com/introduction-to-opengl-and-glsl/#comment-52117)

### Leave a Reply

Your email address will not be published. Required fields are marked *

Comment 

Name*****

Email*****

Website

 Save my name, email, and website in this browser for the next time I comment.



This site uses Akismet to reduce spam. [Learn how your comment data is processed](https://akismet.com/privacy/).

[Privacy and Cookie Policy](https://www.3dgep.com/privacy-and-cookie-policy/) [Proudly powered by WordPress](https://wordpress.org/)