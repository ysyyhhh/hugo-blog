---
title: poetry
date: 2023-11-05
lastmod: 2024-01-04
author: ['Ysyy']
categories: ['']
tags: ['python']
description: 
weight: None
draft: False
comments: True
showToc: True
TocOpen: True
hidemeta: False
disableShare: False
showbreadcrumbs: True
---
## poetry 出现的错误及解决方法

### poetry install 时Failed to create the collection: Prompt dismissed

解决方案: 关闭keyring

```shell
python3 -m keyring --disable
```

原因:
[https://github.com/python-poetry/poetry/issues/1917](https://github.com/python-poetry/poetry/issues/1917)