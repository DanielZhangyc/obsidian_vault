---
date: "2025-03-01T22:55:13+08:00"
tags:
  - 标签1
  - 标签2
title: "VVQuest Extension - 记一次用 AI 开发 Chrome 插件的感受"
slug: "1740840913"
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

[VVQuest](https://github.com/MemeMeow-Studio/VVQuest) 这项目感觉还是需要一个浏览器插件才能发挥，而对于浏览器插件开发又不熟悉，遂尝试用 `Claude 3.7` 开发。

前期的开发过程记录仅供参考。

## 如何开发谷歌插件？

查阅 [Google 提供的文档](https://developer.chrome.com/docs/extensions?hl=zh-cn) 并与LLM对话了解到 Chrome 插件最基础的目录结构

```perl
my-extension/
├── manifest.json     # 插件配置文件（必需）
├── icon.png          # 插件图标
└── popup.html        # 弹出窗口（可选）

```

后续直接在浏览器中加载插件即可

## 明确功能

首先明确接收到用户请求后的基础流程

```mermaid
flowchart TD
    A[接收到用户请求] --> B[处理请求相关, 如获取网页内容]
    B --> C[发送API请求]
    C --> D{API返回信息是图片的地址?}
    D -- 是 --> E[从COS获取图片]
    E --> F[添加到用户剪贴板]
    F --> G[发送消息, 告诉用户处理完成]
    G --> H[处理ratelimit, 进入冷却]
    D -- 否 --> I[处理错误]
    I --> J[返回错误信息]
    J --> K[结束]
```

## 编写提示词

目前的 LLM 受提示词的影响还是较大，且为了使我对其生成的代码可控，我决定让他分步执行操作，并在每一步后进行调试和code review。且我打算采用知识库+Deepseek R1生成合适准确的提示词。

此处就不附上提示词了
