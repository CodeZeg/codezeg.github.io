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

    1. Numbers and Sliders
        1. name ("display name", Range (min, max)) = number
        1. name ("display name", Float) = number
        1. name ("display name", Int) = number

    1. Colors and Vectors
        1. name ("display name", Color) = (number,number,number,number)
        1. name ("display name", Vector) = (number,number,number,number)

    1. Textures
        1. name ("display name", 2D) = "defaulttexture" {}
        1. name ("display name", Cube) = "defaulttexture" {}
        1. name ("display name", 3D) = "defaulttexture" {}

## Subshader

    Subshader { [Tags] [CommonState] Passdef [Passdef ...] }

### SubShader Tags

    1. Queue
        1. Background 1000
        1. Geometry(default) 2000
        1. AlphaTest 2450
        1. Transparent 3000
        1. Overlay 4000
        1. Geometry + 1 = 2001
    1. RenderType
    1. DisableBatching
    1. ForceNoShadowCasting
    1. IgnoreProjector
    1. CanUseSpriteAtlas 
    1. PreviewType 

## Pass

    Pass { [Name and Tags] [RenderSetup] }

### Pass Tags

    1. LightMode 
        1. Always
        1. ForwardBase
        1. ForwardAdd
        1. PrepassBase
        1. PrepassFinal
        1. Vertex
        1. VertexLMRGBM
        1. VertexLM
        1. ShadowCaster
        1. ShadowCollector
    1. RequireOptions 

### Pass RenderSetup

    1. Cull Back | Front | Off
    1. ZTest (Less | Greater | LEqual | GEqual | Equal | NotEqual | Always)
    1. ZWrite On | Off
    1. Offset OffsetFactor, OffsetUnits
    1. Blend sourceBlendMode|destBlendMode
    1. ColorMask RGB | A | 0 | any combination of R, G, B, A
    1. Legacy fixed-function Shader commands 
        1. Lighting On | Off
        1. Material { Material Block }
        1. SeparateSpecular On | Off
        1. Color Color-value
        1. ColorMaterial AmbientAndDiffuse | Emission
        1. Fog { Fog Block }
        1. AlphaTest (Less | Greater | LEqual | GEqual | Equal | NotEqual | Always) CutoffValue
        1. SetTexture textureProperty { combine options }
            1. SetTexture [TextureName] {Texture Block}
            1. combine command
                1. combine src1 * src2
                1. combine src1 + src2
                1. combine src1 - src2
                1. combine src1 lerp (src2) src3 // 如果src2.alpha=1则返回src1, 如果src2.alpha=0则返回src3
                1. combine src1 * src2 + src3
                1. combine RGB , A              // 可以用逗号分隔 RGB 和 Alpha 表达式
            1. combine src
                1. Previous // 上一次SetTexture的结果
                1. Primary // 来自光照计算的颜色或是当它绑定时的顶点颜色
                1. Texture // 在SetTexture中被定义的纹理的颜色
                1. Constant // 被ConstantColor定义的颜色
            1. combine tips
                1. Double or Quad // 调高亮度2倍或4倍
                1. one - // 可以取到负数
                1. alpha  // 只取用alpha通道
            1. ConstantColor color
            1. matrix [MatrixPropertyName]

### UsePass

    1. UsePass "Shader/Name"
    1. Name "MyPassName"

### GrabPass

    1. GrabPass { } 
    1. GrabPass { "TextureName" }

## Fallback

    Fallback "name"