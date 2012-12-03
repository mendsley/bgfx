bgfx
====

What is it?
-----------

Cross-platform rendering library.

Supported rendering backends:

 * OpenGL 2.1
 * OpenGL ES 2
 * OpenGL ES 3
 * Direct3D 9
 * Direct3D 11

Platforms:

 * Windows
 * Linux
 * Android
 * Native Client
 * JavaScript (via Emscripten)

Dependencies
------------

[https://github.com/bkaradzic/bx](https://github.com/bkaradzic/bx)

Optional:  
[https://github.com/mendsley/tinystl](https://github.com/mendsley/tinystl)

Building
--------

### Prerequisites

Premake 4.4 beta4  
[http://industriousone.com/premake/download](http://industriousone.com/premake/download)

GNU make  
Windows users download GNU make utility from:  
[http://gnuwin32.sourceforge.net/packages/make.htm](http://gnuwin32.sourceforge.net/packages/make.htm)

### Getting source

	git clone git://github.com/bkaradzic/bx.git
	git clone git://github.com/bkaradzic/bgfx.git
	cd bgfx
	make

After calling make, .build/projects/* directory will be generated. All
intermediate files generated by compiler will be inside .build directory
structure. Deleting .build directory at any time is safe.

### Building for Linux

	sudo apt-get install libgl1-mesa-dev

### Building for Windows

When building on Windows, you have to set DXSDK_DIR environment variable to
point to DirectX SDK directory.

	setx DXSDK_DIR <path to DirectX SDK directory>

If you're building with MinGW/TDM compiler on Windows make DirectX SDK
directory link to directory without spaces in the path.

	mklink /D <path to DirectX SDK directory> c:\dxsdk
	setx DXSDK_DIR c:\dxsdk

### Building for Native Client (Pepper 22) on Windows

Download Native Client SDK from:  
[https://developers.google.com/native-client/sdk/download](https://developers.google.com/native-client/sdk/download)

	setx NACL <path to Native Client SDK directory>\toolchain\win_x86_newlib

### Building

Visual Studio 2008:

	start .build/projects/vs2008/bgfx.sln

Linux 64-bit:

	make -R linux-release64

Other platforms:

	make -R <configuration>

Configuration is `<platform>-<debug/release><32/64>`. For example:

	linux-release32, nacl-debug64, android-release32, etc.

Examples
--------

### 00-helloworld
Initialization and debug text.

### 01-cubes
Rendering simple static mesh.

![example-01-cubes](https://github.com/bkaradzic/bgfx/raw/master/examples/01-cubes/screenshot.png)

### 02-metaballs
Rendering with transient buffers.

![example-02-metaballs](https://github.com/bkaradzic/bgfx/raw/master/examples/02-metaballs/screenshot.png)

### 03-raymarch
Updating shader uniforms.

![example-03-raymarch](https://github.com/bkaradzic/bgfx/raw/master/examples/03-raymarch/screenshot.png)

### 04-mesh
Loading OpenCTM meshes.

![example-04-mesh](https://github.com/bkaradzic/bgfx/raw/master/examples/04-mesh/screenshot.png)

### 05-instancing
Geometry instancing.

![example-05-instancing](https://github.com/bkaradzic/bgfx/raw/master/examples/05-instancing/screenshot.png)

### 06-bump
Loading textures.

![example-06-bump](https://github.com/bkaradzic/bgfx/raw/master/examples/06-bump/screenshot.png)

Internals
---------

bgfx is using sort-based draw call bucketing. This means that submition order
doesn't necessarily matches the rendering order. On the high level this allows
submitting draw calls for all passes at one place, but on the low-level they
will be sorted and ordered correctly. This sometimes creates undesired results
usually for GUI rendering, where draw order should usually match submit order.
bgfx provides way to enable sequential rendering for these cases (see 
`bgfx::setViewSeq`).

Internally All low-level rendering draw calls are issued inside single function
`Context::rendererSubmit`. This function exist inside each renderer backend
implementation.

More detailed description of sort-based draw call bucketing can be found at:  
[Order your graphics draw calls around!](http://realtimecollisiondetection.net/blog/?p=86)

Customization
-------------

By default each platform has sane default values. For example on Windows default
renderer is DirectX9, and on Linux it is OpenGL 2.1. On Windows platform all
rendering backends are available. For OpenGL ES on desktop you can find more 
information at:  
[OpenGL ES 2.0 and EGL on desktop](http://www.g-truc.net/post-0457.html)

All configuration settings are located inside [src/config.h](https://github.com/bkaradzic/bgfx/blob/master/src/config.h).

Every `BGFX_CONFIG_*` setting can be changed by passing defines thru compiler
switches. For example setting preprocessor define `BGFX_CONFIG_RENDERER_OPENGL=1`
on Windows will change backend renderer to OpenGL 2.1. on Windows. Since
rendering APIs are platform specific, this obviously won't work nor make sense
in all cases. Certain platforms have only single choice, for example the Native
Client works only with OpenGL ES 2.0 renderer, using anything other than that
will result in build errors.

Tools
-----

### Shader Compiler (shaderc)

bgfx cross-platform shader language is based on GLSL syntax. It's uses ANSI C
preprocessor to transform GLSL like language syntax into HLSL. This technique
has certain drawbacks, but overall it's simple and allows quick authoring of
cross-platform shaders.

### Texture Compiler (texturec)

### Geometry Compiler (geometryc)

Todo
----

 - Multiple render targets.
 - BlendFuncSeparate and BlendEquationSeparate.
 - Copy from texture to texture.
 - Occlusion queries.
 - OSX and iOS platforms.
 - DX11: MSAA.
 - GL: MSAA.
 - GL: ARB_vertex_array_object + OES_vertex_array_object.

Notice
------

This is alpha software, and it lacks documentation and examples. If you're
interested to use it in your project, please let me know.

Contact
-------

[@bkaradzic](https://twitter.com/bkaradzic)  
http://www.stuckingeometry.com

Project page  
https://github.com/bkaradzic/bgfx

3rd Party Libraries
-------------------

All required 3rd party libraries are included in bgfx repository in [3rdparty/](3rdparty/)
directory.

### edtaa3 (MIT)

Contour Rendering by Distance Fields

https://github.com/OpenGLInsights/OpenGLInsightsCode/tree/master/Chapter%2012%202D%20Shape%20Rendering%20by%20Distance%20Fields

### fcpp (BSD)

Frexx C preprocessor

https://github.com/bagder/fcpp

### glsl-optimizer (MIT)

GLSL optimizer based on Mesa's GLSL compiler. Used in Unity for mobile shader
optimization.

https://github.com/aras-p/glsl-optimizer

### OpenCTM (Zlib)

OpenCTM � the Open Compressed Triangle Mesh file format � is a
file format, a software library and a tool set for compression of 3D triangle
meshes.

http://openctm.sourceforge.net/

### stb_image (Public Domain)

http://nothings.org/stb_image.c

License (BSD 2-clause)
----------------------

Copyright 2010-2012 Branimir Karadzic. All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

   1. Redistributions of source code must retain the above copyright notice,
      this list of conditions and the following disclaimer.

   2. Redistributions in binary form must reproduce the above copyright notice,
      this list of conditions and the following disclaimer in the documentation
      and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY COPYRIGHT HOLDER ``AS IS'' AND ANY EXPRESS OR
IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO
EVENT SHALL COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT,
INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,
BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF
LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE
OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF
ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
