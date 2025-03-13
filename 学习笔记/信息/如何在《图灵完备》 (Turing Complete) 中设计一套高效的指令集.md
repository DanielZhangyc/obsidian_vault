---
date: "2025-03-13T22:58:21+08:00"
tags:
  - 标签1
  - 标签2
title: "如何在 《图灵完备》 中设计一套高效的指令集"
slug: "1741877901"
share: false
searchHidden: false
disableShare: true
math: false
mermaid: false
canonicalURL: ""
description: ""
series: 系列
lastmod: 
lang: cn
cover.image: ""
author: 
dir: posts
---

《图灵完备》中我们设计出最基础的图灵完备计算机后，需要使用汇编语言对其进行编程，而一套好的指令集是高效编程的基础。

## 问题明确

我们现在对于计算机有如下操作：

```
IMMEDIATE
CALCULATION
COPY
CONDITION
```

共有六个寄存器，编号0-5
`IMMEDIATE`会写入`REM0`
`CALCULATION`可以将`REM1`和`REM2`的值操作后存储到`REM3`中
`CONDIOTON`会判断`REM3`中的值是否满足条件，同时跳转`REM0`中存储的值
`COPY`可以自由复制`REM0`-`REM5`以及`IN`/`OUT`的值

每次输入是一个八位二进制数，划分为 2 + 3 + 3，前两位代表操作：

```
00 -> IMEDIATE
01 -> COMPUTE
10 -> COPY
11 -> CONDITION
```

`IMMEDIATE`模式下，后六位数值即为读入数值；

`CALCULATION`模式下主要读入最后三位，有如下对应：

```
000 -> OR
001 -> NAND
010 -> NOR
011 -> AND
100 -> ADD
101 -> SUB
```

`CONDITION`模式下主要读入最后三位，有如下对应：

```
000 -> NEVER
001 -> =0
011 -> <=0
100 -> ALWAYS
101 -> !=0
110 -> >=0
111 -> >0
```

`COPY`将3-5位对应寄存器数据复制到6-8位对应寄存器，数值对应寄存器编号，且数值为6时为 `IN`/`OUT`

目前一个别名仅能对应一个二进制数字