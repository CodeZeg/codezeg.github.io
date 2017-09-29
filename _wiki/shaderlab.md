---
layout: wiki
title: shaderlab
description: unity shader 语法
keywords: shader, unity
---

# 语法

## Shader

    Shader "name" { [Properties] Subshaders [Fallback] [CustomEditor] }

## Properties

    Properties { Property [Property ...] }

+ Numbers and Sliders
    + name ("display name", Range (min, max)) = number
    + name ("display name", Float) = number
    + name ("display name", Int) = number

+ Colors and Vectors
    + name ("display name", Color) = (number,number,number,number)
    + name ("display name", Vector) = (number,number,number,number)

+ Textures
    + name ("display name", 2D) = "defaulttexture" {}
    + name ("display name", Cube) = "defaulttexture" {}
    + name ("display name", 3D) = "defaulttexture" {}

## Subshader

    Subshader { [Tags] [CommonState] Passdef [Passdef ...] }

### SubShader Tags

+ Queue
    + Background 1000
    + Geometry(default) 2000
    + AlphaTest 2450
    + Transparent 3000
    + Overlay 4000
    + Geometry + 1 = 2001
+ RenderType
+ DisableBatching
+ ForceNoShadowCasting
+ IgnoreProjector
+ CanUseSpriteAtlas 
+ PreviewType 

## Pass

    Pass { [Name and Tags] [RenderSetup] }

### Pass Tags

+ LightMode 
    + Always
    + ForwardBase
    + ForwardAdd
    + PrepassBase
    + PrepassFinal
    + Vertex
    + VertexLMRGBM
    + VertexLM
    + ShadowCaster
    + ShadowCollector
+ RequireOptions 

### Pass RenderSetup

+ Cull Back | Front | Off          `剔除掉  背离观察者 | 面向观察者 | 关闭`
+ ZTest (Less | Greater | LEqual | GEqual | Equal | NotEqual | Always)
+ ZWrite On | Off
+ Offset Factor, Units
+ Blend sourceBlendMode | destBlendMode
+ ColorMask RGB | A | 0 | any combination of R, G, B, A
+ Legacy fixed-function Shader commands 
    + Lighting On | Off
    + Material { Material Block }
        + Diffuse Color            `漫反射`
        + Ambient Color            `环境色`
        + Specular Color           `高光色`
        + Shininess Number         `光泽度`
        + Emission Color           `自发光`
        + FinalColor = Ambient * ambientsetting + (Light Color * Diffuse + Light Color *Specular) + Emission
    + SeparateSpecular On | Off
    + Color Color-value
    + ColorMaterial AmbientAndDiffuse | Emission
    + Fog { Fog Block }
        + Mode Off | Global | Linear | Exp | Exp2
        + Color ColorValue
        + Density FloatValue
        + Range FloatValue , FloatValue
    + AlphaTest (Less | Greater | LEqual | GEqual | Equal | NotEqual | Always) CutoffValue
    + SetTexture textureProperty { combine options }
        + SetTexture [TextureName] {Texture Block}
        + combine command
            + combine src1 * src2
            + combine src1 + src2
            + combine src1 - src2
            + combine src1 lerp (src2) src3 `如果src2.alpha=1则返回src1, 如果src2.alpha=0则返回src3`
            + combine src1 * src2 + src3
            + combine RGB , A              `可以用逗号分隔 RGB 和 Alpha 表达式`
        + combine src
            + Previous `上一次SetTexture的结果`
            + Primary `来自光照计算的颜色或是当它绑定时的顶点颜色`
            + Texture `在SetTexture中被定义的纹理的颜色`
            + Constant `被ConstantColor定义的颜色`
        + combine tips
            + Double or Quad `调高亮度2倍或4倍`
            + one - `可以取到负数`
            + alpha  `只取用alpha通道`
        + ConstantColor color
        + matrix [MatrixPropertyName]

### UsePass

+ UsePass "Shader/Name"
+ Name "MyPassName"

### GrabPass

+ GrabPass { } 
+ GrabPass { "TextureName" }

## Fallback

    Fallback "name"