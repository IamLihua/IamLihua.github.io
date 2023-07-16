---
title: 关于python -m visdom.server缓慢
date: 2023-07-15 16:00:09
tags: torch
categories: 
  - bug
---

# 解决`python -m visdom.server`缓慢的问题

## 问题描述

使用`python -m visdom.server`后没反应

```
E:\pprogram\test>python -m visdom.server
Checking for scripts.
Downloading scripts, this may take a little while
```

## 解决方案

下载这个文件夹中的所有文件[visdom/py/visdom/static at master · fossasia/visdom (github.com)](https://github.com/fossasia/visdom/tree/master/py/visdom/static)

去替换conda库环境下的文件夹，如我这里是`D:\Anaconda3\envs\torch\Lib\site-packages\visdom\static`

对于版本为`0.2.4` 可以去找`D:\Anaconda3\envs\torch\Lib\site-packages\visdom\server\run_server.py`这个文件

靠前一些的版本如果没有这个文件，可以去找找看有没有叫`server.py`的文件

找到这里，如我这里是在235行，将其注释掉

```python
def download_scripts_and_run():
  # download_scripts()
  main()
```

然后就重新开始就好了

```text
(torch) E:\pprogram\test>python -m visdom.server
It's Alive!
INFO:root:Application Started
INFO:root:Working directory: C:\Users\Administrator\.visdom
You can navigate to http://localhost:8097
```

