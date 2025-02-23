---
date: 2025-02-23T13:40:21+08:00
tags:
  - Python
title: 学习笔记 | Python 中 ABC (抽象基类) 的使用
slug: "1740289221"
share: true
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

## 原代码痛点

在项目开发中需要实现代码的重构，原先嵌入模型使用 API / 本地的方法放在一个类中实现，通过 `self.mode` 进行控制切换。为了方便后续维护，需要对本地和 API 的类分离

```Python
class EmbeddingService:
	def __init__(self):
		...
		self.mode = 'api' # 'api' or 'local'

	def _get_embedding_cache(self):
		if(self.model == 'api'):
			# api
		else:
			# local
```

## 重构方案

我们首先定义基类  `BaseEmbeddingService` :

```Python
from abc import ABC, abstractmethod

class BaseEmbeddingService(ABC):
	pass
```

在基类之中，可使用 `abstractmethod` 修饰器包裹子类继承时需要重写的方法，也就是表示这是一个抽象方法。

{{< notice info >}}
抽象方法无法被实例化，实例化将会直接报错。
{{< /notice >}}


```Python
class BaseEmbeddingService(ABC):
	def __init__(self):
        self.embedding_cache: Dict[str, Dict[str, np.ndarray]] = {}
        self._get_embedding_cache()

	@abstractmethod
    def _get_embedding_cache(self) -> None:
        """获取嵌入缓存,子类必须实现"""
        pass
	...

class LocalEmbeddingService(BaseEmbeddingService): # 继承自基类
	def __init__(self, model_name: Optional[str] = None):
        super().__init__()
        self.local_models = {}
        self.current_model = None
        self.selected_model = model_name or Config().models.default_model
        if self.is_model_downloaded(self.selected_model):
            try:
                self._load_model(self.selected_model)
            except Exception as e:
                print(f"模型加载失败: {str(e)}")

	def _get_embedding_cache(self) -> None: # 重写抽象方法
        """获取本地模型的嵌入缓存"""
        if not self.selected_model:
            return
        cache_file = Config().get_absolute_cache_file().replace('.pkl', f'_{self.selected_model}.pkl')
        verify_folder(cache_file)
        if os.path.exists(cache_file):
            with open(cache_file, 'rb') as f:
                self.embedding_cache = pickle.load(f)
	...
```

## 为什么需要使用ABC？

对比其他方案：

|方案|扩展成本|类型安全|代码复用|可维护性|
|---|---|---|---|---|
|原始if-else模式|高|无|低|差|
|策略模式|中|部分|中|一般|
|**ABC抽象基类**|低|强|高|优秀|

当子类为实现抽象方法时将会**立即报错**，安全性高。可统一所有实现的**方法签名**