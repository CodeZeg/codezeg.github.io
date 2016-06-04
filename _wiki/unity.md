---
layout: wiki
title: Unity
categories: [cate1, cate2]
description: Unity的一些注意事项
keywords: Unity, C#
---

## 语法规范

### for vs. foreach 

1. Unity编译的Dll使用foreach会产生额外的GC
2. VS编译的Dll不会
3. 数组遍历的效率比enumerator高约5倍

### lambda 表达式 vs. 闭包 (closure)

1. 引用了外部变量的 lambda 表达式就是闭包
2. 闭包会把引用的外部变量保存在内存中
3. 所以使用闭包时要注意清理外部变量，避免内存泄漏

### 枚举项的 ToString() vs. Enum.GetName()

1. 前者耗时是后者的接近两倍
2. 尽量使用后者

### 使用 "as" 转型 vs. 使用 C-Style Cast

1. as 就是加入了判空的 cast
2. 看情况选用

### 其他

- 利用 string 的 immutable 特性，在内存中单一实例 (Interning) 的特性
- 利用 string 的比较性能好 (当引用方式为 object 时进行地址比较) 的特性
- 在需要时使用 StringBuilder
- 利用好容器的 Capacity 来优化内存访问
- 利用 ref 和 struct 来把堆 (heap) 上的访问往栈 (stack) 上挪
- 避免使用 LINQ 来降低零碎的内存分配

## 实践中的 GC 控制手法

从实践上看，与 GC 相关的控制手法主要是以下这些：

1. 避免无谓的反复分配，尤其是隐含的每帧分配
	典型的例子是在 Update() 函数里面拼接字符串

2. 在可能的时刻主动触发 GC，这些时刻包括：
     刚刚进入某张地图时
     刚刚打开某个(静态)界面时
     结束掉某一段剧情/新手引导时

3. 使用对象池策略性地重用对象
     把对象的引用归还到对象池，主动有计划地持有引用，而非交给 GC
     做好平衡和取舍（最小化分配/释放的行为，同时妥善考虑内存占用量的调整）

4. 在 GC.Collect() 之前，确保置空所有能被清理的对象，以最大化 GC.Collect() 运行一次的性价比

- 2 和 3 的意义在于，对于每次 GC 而言，如果没有需要释放的对象，速度会非常快。
- 可以连续触发多帧的 GC ，就能在 Profiler 中看到，时间消耗的峰值就是第一次 GC。
- 所以尽量手动 GC 的好处就是，会降低 GC 发生在你不期望的时间的几率，也能降低万一发生时的时间开销。

