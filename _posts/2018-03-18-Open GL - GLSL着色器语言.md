---
layout: post
title: "Open GL - GLSL着色器语言"
date: 2018-03-18 
description: "------"
tag: OpenGL 
--- 


GLSL(OpenGL Shading Language) 是一个以C语言为基础的高阶着色语言


## 作用

可用于 OpenGL 可编程管线
能够让你对OpenGL渲染其中的一些着色环节来自定义（如顶点着色器，片元着色器）
类似UIVIewController的自定义控件

固定管线
相当于已经封装了的高级API，让你通过传递参数来实现效果

## 自定义环节
* 顶点着色器： 处理每个顶点，确定位置以及变换
* 片元着色器： 片元

## GLSL 数据类型

* void – 用于没有返回值的函式
* bool – 条件类型，其值可以是真或假
* int – 带负号整数
* float – 浮点数
* vec2 – 2 个浮点数组成的向量
* vec3 – 3 个浮点数组成的向量
* vec4 – 4 个浮点数组成的向量
* bvec2 – 2 个布尔组成的向量
* bvec3 – 3 个布尔组成的向量
* bvec4 – 4 个布尔组成的向量
* ivec2 – 2 个整数组成的向量
* ivec3 – 3 个整数组成的向量
* ivec4 – 4 个整数组成的向量
* mat2 – 浮点数的 2X2 矩阵
* mat3 – 浮点数的 3X3 矩阵
* mat4 – 浮点数的 4X4 矩阵
* sampler1D – 用来存取一维纹理的句柄（handle）（或：操作，作名词解。）
* sampler2D – 用来存取二维纹理的句柄
* sampler3D – 用来存取三维纹理的句柄
* samplerCube – 用来存取立方映射纹理的句柄
* sampler1Dshadow – 用来存取一维深度纹理的句柄
* sampler2Dshadow – 用来存取二维深度纹理的句柄


##  存储修饰符

**1. 常量修饰符 const**

    1. 任何使用const声明的变量在其所属的着色器中均是只读的。
    2. const 用来修饰任何基本数据类型
    3. const 不能用来修饰包含数组的数组、结构体
    
 **2. Attribute**
    
    1. 用于修饰声明通过OpenGL ES 应用程序传递顶点着色器的变量值，在其他任何非顶点着⾊器的着色器程序中声明attribute变量是错误的

**3. Uniform**
    
    1. ⽤来修饰那些被整个图元在被处理过程中保持不变的全局变量
    2. 所有uniform变量都是只读的
    3. uniform修饰符可以和任意基本数据类型一起使用，或者包含基本数据类型元素的数组和结构体
 
**4. Varying**

    1. varying变量提供了顶点着⾊器，⽚元着色器和二者通讯控制模块之间的接口
    2. 顶点着⾊器计算每个顶点的值(颜⾊，纹理坐标等)并将它们写到 varying 变量中。顶点着⾊器也会从 varying 变量中读值，获取和它写入相同的值
    3. ⽚元着⾊器会读取varying变量的值，并且被读取的值将会作为插值器，作为图元中片元位置的一个功能信息。varying 变量对于⽚元着⾊器来说是只读的。

##  精度修饰符

**highp**：  浮点数的范围 (-2^62 - 2^62) ，整型范围 (-2^16 - 2^16)
**mediump**：浮点数的范围 (-2^14 - 2^14)， 整型范围 (-2^10 - 2^10)
**lowp**：   浮点数的范围 (-2,2)，         整型范围(-2^8 - 2^8)



## 官方的shader范例:

### Vertex Shader:

```
uniform mat4 mvp_matrix; //透视矩阵 * 视图矩阵 * 模型变换矩阵
uniform mat3 normal_matrix; //法线变换矩阵(用于物体变换后法线跟着变换)
uniform vec3 ec_light_dir; //光照方向
attribute vec4 a_vertex; // 顶点坐标
attribute vec3 a_normal; //顶点法线
attribute vec2 a_texcoord; //纹理坐标
varying float v_diffuse; //法线与入射光的夹角
varying vec2 v_texcoord; //2d纹理坐标
void main(void)
{
 //归一化法线
 vec3 ec_normal = normalize(normal_matrix * a_normal);
 //v_diffuse 是法线与光照的夹角.根据向量点乘法则,当两向量长度为1是 乘积即cosθ值
 v_diffuse = max(dot(ec_light_dir, ec_normal), 0.0);
 v_texcoord = a_texcoord;
 gl_Position = mvp_matrix * a_vertex;
}
```

### Fragment Shader:

```
precision mediump float;
uniform sampler2D t_reflectance;
uniform vec4 i_ambient;
varying float v_diffuse;
varying vec2 v_texcoord;
void main (void)
{
 vec4 color = texture2D(t_reflectance, v_texcoord);
 //这里分解开来是 color*vec3(1,1,1)*v_diffuse + color*i_ambient
 //色*光*夹角cos + 色*环境光
 gl_FragColor = color*(vec4(v_diffuse) + i_ambient);
}
```


## 目录
[toc]





