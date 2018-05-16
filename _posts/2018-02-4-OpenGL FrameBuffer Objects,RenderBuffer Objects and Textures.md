---
layout: post
title: "OpenGL FrameBuffer Objects,RenderBuffer Objects and Textures"
date: 2018-02-04 
description: "------"
tag: OpenGL 
--- 



### RenderBuffer
 > A renderbuffer object is a 2D image buffer allocated by the application. The renderbuffer can be used to allocate and store color, depth, or stencil values and can be used as a color, depth, or stencil attachment in a framebuffer object. A renderbuffer is similar to an off-screen window system provided drawable surface, such as a pbuffer. A renderbuffer, however, cannot be directly used as a GL texture.
 


> 渲染缓冲区对象是由应用程序分配的2D图像缓冲区。渲染缓冲区可用于分配和存储颜色，深度或模板值，并可用作帧缓冲区对象中的颜色，深度或模板附件。渲染缓冲器类似于提供可绘制表面的离屏窗口系统，例如pbuffer。但是，渲染缓冲区不能直接用作GL纹理。



### FrameBuffer

> A framebuffer object (often referred to as an FBO) is a collection of color, depth, and stencil buffer attachment points; state that describes properties such as the size and format of the color, depth, and stencil buffers attached to the FBO; and the names of the texture and renderbuffer objects attached to the FBO. Various 2D images can be attached to the color attachment point in the framebuffer object. These include a renderbuffer object that stores color values, a mip-level of a 2D texture or a cubemap face, or even a mip-level of a 2D slice in a 3D texture. Similarly, various 2D images contain- ing depth values can be attached to the depth attachment point of an FBO. These can include a renderbuffer, a mip-level of a 2D texture or a cubemap face that stores depth values. The only 2D image that can be attached to the stencil attachment point of an FBO is a renderbuffer object that stores stencil values.

> ⼀个 frameBuffer 对象(通常被称为⼀个FBO)。是一个收集颜色、深度和模板缓存区的附着点。描述属性的状态，例如颜⾊、深度和模板缓存区的大小和格式，都关联到FBO(Frame Buffer Object)。并且纹理的名字和 renderBuffer 对象也都是关联于FBO。各种各样的2D图形能够被附着framebuffer对象的颜色附着点。它们包含了renderbuffer对象存储的颜⾊色值、⼀个2D纹理或⽴方体贴图。或者⼀一个mip-level的二维切面在 3D纹理。同样，各种各样的2D图形包含了当时的深度值可以附加到⼀个FBO的深度附着点中去。唯⼀的⼆维图像，能够附着在FBO的模板附着点，是一个renderbuffer对象存储模板值。


![](/images/media/15264553946043.jpg)






