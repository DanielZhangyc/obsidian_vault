---
date: 2025-02-08T00:10:50+08:00
tags: []
title: OI | ABC 391 E 题解整理
slug: "1738944650"
share: true
searchHidden: false
disableShare: true
math: true
mermaid: false
canonicalURL: ""
keywords: 
description: ""
series: 系列
lastmod: 
lang: cn
cover.image: ""
author: 
dir: posts
---
## 题面

>For a binary string $B = B_1 B_2 \dots B_{3^n}$ of length $3^n$ ($n \geq 1$), we define an operation to obtain a binary string $C = C_1 C_2 \dots C_{3^{n-1}}$ of length $3^{n-1}$ as follows:
>
>-   Partition the elements of $B$ into groups of $3$ and take the majority value from each group. That is, for $i=1,2,\dots,3^{n-1}$, let $C_i$ be the value that appears most frequently among $B_{3i-2}$, $B_{3i-1}$, and $B_{3i}$.
>
>You are given a binary string $A = A_1 A_2 \dots A_{3^N}$ of length $3^N$. Let $A' = A'_1$ be the length-$1$ string obtained by applying the above operation $N$ times to $A$.
>
  Determine the minimum number of elements of $A$ that must be changed (from $0$ to $1$ or from $1$ to $0$) in order to change the value of $A'_1$.

简单来说，给定01字符串 $A$ ，长度为 $3^n$ ，每次操作将字符串分割为三个为一组，取出现最多的数替代这三个数字，要使最终得到的字符串改变结果 $(0 \rightarrow 1 \ or \ 1 \rightarrow 0)$  ，最少需要改变几个数字

## 思路分析

首先进行分析 ，不难发现只有 $0,1,2,4$ 经过操作后得到的是 $0$ ，其余情况得到的均为 $1$ 。要将三个一组的数经过操作得到的结果变为另一种，需要改变1/2个数字，其中改变2个数字的有 $000, 111$ 其余情况均只需改变一个数字。我们可以定义要改变的数字数量为代价，考虑建图。

## 解题思路

可将字符串 $A$ 视为三叉树的叶子结点，对每个节可定义一个 $Cost$ ，代表改变为另一种数字所需的代价。