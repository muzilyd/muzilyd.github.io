---
redirect_from: /_posts/2021-12-31-Jetson agx xavier刷机+pcle固态加装+pytorch+miniforge(anaconda)+vscode+wifi+ros全套安装(亲测有效，完美通过无需踩坑).md/
title: Jetson agx xavier刷机+pcle固态加装+pytorch+miniforge(anaconda)+vscode+wifi+ros全套安装(亲测有效，完美通过无需踩坑)
tags: Jetson agx xavier
---

### 一、Jetson agx xavier刷机
## 1、开始之前一定要先换源，然后更新
修改sources.list
```bash
sudo gedit /etc/apt/sources.list
```
将里面的文件换成以下的任意一个内容（哪个好用用哪个）
清华源
```bash
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-updates main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-security main restricted universe multiverse
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-security main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-backports main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic main universe restricted
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic main universe restricted
```
中科大源
```bash
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic-updates main restricted universe multiverse 
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic-updates main restricted universe multiverse 
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic-security main restricted universe multiverse 
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic-security main restricted universe multiverse 
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic-backports main restricted universe multiverse 
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic-backports main restricted universe multiverse 
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic main universe restricted 
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic main universe restricted
```

更新升级package
```bash
sudo apt-get update
```
```bash
sudo apt-get upgrade
```

## 2、下载安装包（这里是4.6版本，建议下载最新的稳定版本）
[链接](https://developer.nvidia.com/embedded/jetpack)

![sdkmanager](https://raw.githubusercontent.com/muzilyd/blog-image/main/jetson%20agx%20xavier/sdkmanager.png)

## 3、Jetson agx xavier安装sdkmanagerimage
```bash
sudo apt install ./sdkmanager_1.6.1-8175_amd64
```

## 4、成功之后在终端中打开sdkmanger
```bash
sdkmanager  #直接输入sdkmanager就行
```

