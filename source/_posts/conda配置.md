---
title: conda配置
date: 2023-04-10 15:44:33
tags: conda
categories: config
excerpt: 实现conda换源配置和复制已经环境的方法
---

## Windows系统下

```cmd
conda config --set show_channel_urls yes
```

用这段命令在用户文件夹下生成`.condarc`文件

## 其他系统下

直接就有`.condarc`文件，可以直接修改

## 修改`.condarc`文件

根据[镜像站使用帮助 | 清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/)

复制以下内容进去,`CtrlA`+`CtrlV`

```condarc
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  deepmodeling: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/
```

运行 `conda clean -i` 清除索引缓存，保证用的是镜像站提供的索引。

亲测不会卡进度。

## 复制已有环境

```sh
conda create -n new_env --clone exist_env
```

## 安装miniconda命令

```sh
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
```

然后`yes -> enter -> yes`

如果不想直接进入`base`环境，就输入以下命令：

```sh
conda config --set auto_activate_base false
```

## apt换源命令

再补一个apt换源方法：[Ubuntu 20.04 && Ubuntu 18.04 修改 apt 源_ubuntu 20.04 && ubuntu18.04 xiu gai-CSDN博客](https://blog.csdn.net/WU2629409421perfect/article/details/110881141)
