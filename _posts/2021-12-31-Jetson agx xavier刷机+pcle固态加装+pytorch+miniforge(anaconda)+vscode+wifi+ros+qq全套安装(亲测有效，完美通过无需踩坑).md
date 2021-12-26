---
redirect_from: /_posts/2021-12-31-Jetson agx xavier刷机+pcle固态加装+pytorch+miniforge(anaconda)+vscode+wifi+ros+qq全套安装(亲测有效，完美通过无需踩坑).md/
title: Jetson agx xavier刷机+pcle固态加装+pytorch+miniforge(anaconda)+vscode+wifi+ros+qq全套安装(亲测有效，完美通过无需踩坑)
tags: Jetson-agx-xavier
---

### 一、Jetson agx xavier刷机
## 1、准备工作
1. 首先你在自己的电脑下载vm安装虚拟机unbantu18.04/16.04(最好是18.04)（选择虚拟机内存的时候建议选择60g内存和4g运行内存）
2. 用usb3.0口将你的电脑连接jetson板电源指示灯处type-C（一般买jetson-agx-xavier都会赠送线）
3. 准备一根网线，将你的电脑和jetson-agx-xavier相连使两个设备连接到一个局域网之内（想要采用这种方法让agx使用电脑的WiFi的话可以参考这个[链接](https://blog.csdn.net/Baofu_Wu/article/details/105920335?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-3.opensearchhbase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-3.opensearchhbase)）
4. 准备一套显示屏，鼠标，键盘给jetson-agx-xavier使用，后面要对agx进行配置需要用到

## 2、开始之前一定要先换源，然后更新
修改sources.list
```bash
sudo gedit /etc/apt/sources.list
```
将里面的文件换成以下的任意一个内容（哪个好用用哪个）（这里提供其他的源如果想要更换你们可以自行提取[链接](https://blog.csdn.net/xiangxianghehe/article/details/80112149)）

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

## 3、下载安装包（这里是4.6版本，建议下载最新的稳定版本）
[安装包下载链接](https://developer.nvidia.com/embedded/jetpack)

![sdkmanager](https://raw.githubusercontent.com/muzilyd/blog-image/main/jetson%20agx%20xavier/sdkmanager.png)

## 4、Jetson agx xavier安装sdkmanagerimage
```bash
sudo apt install ./sdkmanager_1.6.1-8175_amd64
```

## 5、成功之后在终端中打开sdkmanger
```bash
sdkmanager  #直接输入sdkmanager就行
```
## 6、第一步：不要选host会影响你的下载速度（这个选项就是同时下载文件到你的虚拟机里，完全没必要）
![sdkmanager1](https://raw.githubusercontent.com/muzilyd/blog-image/main/jetson%20agx%20xavier/sdkmanager1.png)

## 7、第二步：点先下载再安装按钮速度会更快，可以自己更换路径（我反正使默认路径）然后点continue
![sdkmanager2](https://raw.githubusercontent.com/muzilyd/blog-image/main/jetson%20agx%20xavier/sdkmanager2.png)

## 8、第三步：已经开始下载了，期间会跳出很多的选项
<br/>(1) 25%左右后面会让你选择手动运行（automatic setup）还是自动运行（manual setup），一定要选择手动运行（automatic setup）亲测自动运行根本行不通</br>
- 在此之前我们已经使用了type-c转usb数据线将电脑和agx连接了起来（可以在虚拟机里使用lsusb查看是否连接上）
- 将 Xavier 插上电源，并处于关机状态
- 点击Flash，准备刷机
- 按下并保持agx上的Recovery键（中间的键）
- 按下并保持Power键（最左边的键），持续1s，然后同时松开这两个键，进入刷机模式。

<br/>(2) 50%左右（各个人不一样）agx自动开机，需要重新进行配置，然后记住你设置的账号密码地址，后面虚拟机上需要输入（如果agx地址没有自动显示可以再agx终端上使用ifconfig命令查看）</br>
![sdkmanager3](https://raw.githubusercontent.com/muzilyd/blog-image/main/jetson%20agx%20xavier/sdkmanager3.png)
<br/>(3) 经过漫长的等待之后，出现下列图标，恭喜你，你成功了</br>
![sdkmanager4](https://raw.githubusercontent.com/muzilyd/blog-image/main/jetson%20agx%20xavier/sdkmanager4.png)

## 9、基本配置
1. 设置高性能模式
```bash
sudo nvpmodel --query
```
```bash
sudo nvpmodel -m 0
```
2. 开启风扇（开机一定要开启风扇不然agx会很烫）
```bash
sudo sh -c "echo 150 > /sys/devices/pwm-fan/target_pwm"
```
## 10、在刷机完成之后最好再重新进行一下第2步：换源更新

### 二、Jetson agx xavier pcie固态加装
1. 买一块M.2KeySSD的固态
2. 将agx倒过来把两个支架拆开
3. 用卡片或者小针伸进板子和壳之间的缝里撬开它，他们之间主要就是依靠下图的这些卡口连在了一起所以直接撬开就行，**注意撬开后一定要慢慢拿起**，里面有一根风机线很细容易断
![agxpcie](https://raw.githubusercontent.com/muzilyd/blog-image/main/jetson%20agx%20xavier/agxpcie.png)
4. 将买的固态插进槽内</br>
![pcie](https://raw.githubusercontent.com/muzilyd/blog-image/main/jetson%20agx%20xavier/pcie.png)
5. 装好固态后，将其他的地方也重新装好，将agx开机
6. SSD挂载（[原链接](https://www.jianshu.com/p/045df333042e?from=singlemessage)）提取了其中的精华
<br/>(1) NVMe SSD硬盘仅作为系统盘（rootfs和用户区），系统的启动引导依然是通过SD卡或EMMC，比如升级设备树dtb 还是在SD卡或EMMC中</br>
<br/>(2) 准备M.2 Key M SSD</br>
<br/>(3) 打开Ubuntu18.04自带 Disks 工具，'Ctrl+F' 或点击右上角选择‘Format Disk' 并将其格式化为GPT 格式(可以在搜索中搜到disks工具)</br>
<br/>(4) 格式化时必须选择“Ext4”， 等待完成后，点击下方 '三角按钮'，mount 到固定目录如/media/nvidia/xxxx</br>
<br/>(5) 参考下图:</br>
![pcie1](https://raw.githubusercontent.com/muzilyd/blog-image/main/jetson%20agx%20xavier/pcie1.webp)
![pcie2](https://raw.githubusercontent.com/muzilyd/blog-image/main/jetson%20agx%20xavier/pcie2.webp)
![pcie3](https://raw.githubusercontent.com/muzilyd/blog-image/main/jetson%20agx%20xavier/pcie3.webp)
<br/>(6) 在终端依次中输入</br>
```bash
git clone https://github.com/jetsonhacks/rootOnNVMe.git
```
```bash
cd rootOnNVMe
```
```bash
./copy-rootfs-ssd.sh
```
```bash
./setup-service.sh
```
```bash
reboot
```
<br/>(7) 去home查看属性就可以看到你的内存已经变多啦！</br>

### 三、Jetson agx xavier wifi安装
1. wifi我使用的使用的是intel ax200蓝牙/wifi模块网上都能买得到，一定要买ax200，我之前想要选用更高级的型号可能是内核跟不上无法使用，所以不要图新
![ax200](https://raw.githubusercontent.com/muzilyd/blog-image/main/jetson%20agx%20xavier/ax200.png)
2. 由于我每一个方法都试了最后也不知道到底是哪个方法让我的wifi好了起来这里就从易到难给大家排序以下，你们一个个试试，看第几个好用（[链接1](https://blog.csdn.net/weixin_38693938/article/details/107997558?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163927502716780264059950%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=163927502716780264059950&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-3-107997558.pc_search_mgc_flag&utm_term=agx+xavier+%E6%97%A0%E7%BA%BF%E9%A9%B1%E5%8A%A8&spm=1018.2226.3001.4187)，[链接2](https://blog.csdn.net/haigujiujian/article/details/114914875?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-3.no_search_link&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-3.no_search_link)，[链接3](https://blog.csdn.net/qq_45067735/article/details/113405263?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-3.no_search_link&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-3.no_search_link)）
3. 大家最后可以在文章最后的评论区告诉我到底是哪一个好用哦！！

### 四、Jetson agx xavier miniforge(anaconda)安装
1. 首先去[githubminiforge链接](https://github.com/conda-forge/miniforge)处下载压缩包
![miniforge1](https://raw.githubusercontent.com/muzilyd/blog-image/main/jetson%20agx%20xavier/miniforge1.png)
![miniforge2](https://raw.githubusercontent.com/muzilyd/blog-image/main/jetson%20agx%20xavier/miniforge2.png)
2. 然后使用命令安装
```bash
sh Miniforge-pypy3-4.8.3-4-Linux-aarch64.sh
```
3. 然后切换源(任选其一)
```bash
# 这里使用国科大镜像源
conda config --prepend channels https://mirrors.ustc.edu.cn/anaconda/pkgs/main/
conda config --prepend channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/
conda config --set show_channel_urls yes
```

```bash
# 清华镜像源
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
```
执行完以上命令会在当前用户目录下生成一个.condarc文件，运行cat ~/.condarc命令查看文件内容(一定要把- defaults删除掉,博主就是因为没有删除defaults后面创建虚拟环境有很多错)：
```bash
channels:
  - https://mirrors.ustc.edu.cn/anaconda/pkgs/free/
  - https://mirrors.ustc.edu.cn/anaconda/pkgs/main/
  - defaults
show_channel_urls: true
```
4. 添加环境变量</br>
在环境配置文件里加一个alias，首先编辑vim ~/.bashrc，添加如下内容：alias sudo="sudo env PATH=$PATH"，最后执行source ~/.bashrc使新配置的内容生效。
5. 运行命令conda即可查看当前conda版本
6. 然后别忘了更新conda
```bash
conda update --all #更新所有库
```
8. 在conda中创建虚拟环境（为后面安装pytorch做准备，pytorch最好用python3.6，所以创建一个python3.6的虚拟机）
```bash
conda create -n pytorch python=3.6
```
7. 执行conda activate pytorch命令即可变为pytorch环境

### 五、Jetson agx xavier pytorch安装
1. 使用常规的anaconda方法无法安装pytorch，Nvidia官方为我们提供了.whl文件，[官网链接](https://forums.developer.nvidia.com/t/pytorch-for-jetson-version-1-10-now-available/72048)(因为是外网，建议翻墙打开)
2. 然后打开刚刚**创建的python3.6的环境**，一定要打开这个不然后面下载会报错，python3.6对应的就是后面安装包里的**cp36**，需要其他python包版本的可以自行查看
3. 根据自己的JetPack版本和Python版本，从Nvidia官网选择所需要的PyTorch进行下载，如下图所示：</br>
![pytorch](https://raw.githubusercontent.com/muzilyd/blog-image/main/jetson%20agx%20xavier/pytorch.png)
3. 下载完成之后，按以下程序进行安装下载：
```bash
sudo apt-get install python3-pip libopenblas-base libopenmpi-dev 
pip3 install Cython
pip3 install numpy  torch-1.10.0-cp36-cp36m-linux_aarch64.whl
```
4. 安装完毕后可以在终端输入以下命令检验PyTorch是否正确安装：
```bash
python3 -c 'import torch; print(torch.cuda.is_available())' #如果成功了就会返回TRUE
```
5. 如果不是TRUE，出现了Illegal instruction (core dumped)错误，这是因为numpy 1.19.5和OpenBLAS冲突可以采用以下两种方法来解决：</br>
(1) 降低numpy版本
```bash
pip3 install -U "numpy==1.19.4"
```
(2)设置OpenBLAS
```bash
vim ~/.bashrc
export OPENBLAS_CORETYPE=ARMV8
source ~/.bashrc
```
6. 选择与 pytorch 版本对应的 torchvision,自己去[链接](https://github.com/pytorch/vision#installation)查询pytorch对应torchvision版本,**一定要改为所需要的版本号**
```bash
sudo apt-get install libjpeg-dev zlib1g-dev
git clone --branch <version> https://github.com/pytorch/vision torchvision   # 将‘ <version> ’改为所需要的版本号
cd torchvision
sudo python setup.py install
export BUILD_VERSION=<version>
cd ../  # attempting to load torchvision from build dir will result in import error
#pip install 'pillow<7' # always needed for Python 2.7, not needed torchvision v0.5.0+ with Python 3.6
```
7. 安装成功之后就可以调用pytorch敲代码啦!

### 六、Jetson agx xavier ros安装
1. Xavier不能够用常规方法安装ROS，需要通过ROSXavier进行安装，安装步骤十分简单：
```bash
git clone https://github.com/jetsonhacks/installROSXavier.git
cd installROSXavier
./installROSXavier
```
2. 安装好后在终端输入roscore检查能否正常启动，如果不能，则用以下命令打开.bashrc文件添加环境变量
```bash
sudo gedit ~/.bashrc 
```
3. 文件的最后面加上
```bash
export LD_LIBRARY_PATH=/opt/ros/melodic/lib
export LC_ALL="C"
```
4. 进行更新
```bash
sudo apt update
sudo apt install ros-melodic-desktop-full
```

### 七 Jetson agx xavier vscode安装
1. 可以先去[vscode官网链接](https://code.visualstudio.com/docs/?dv=linuxarm64_deb)下载安装包,选择arm64，deb格式，安装包的格式：code_1.63.2-1639561157_arm64
![vscode](https://raw.githubusercontent.com/muzilyd/blog-image/main/jetson%20agx%20xavier/vscode.png)
2. 也可用下面的命令自动下载安装包：
```bash
wget https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-arm64
```
3. 下载好后安装
```bash
sudo dpkg -i code_1.57.1-1623936438_arm64.deb
```
4. 输入下面的代码即可打开
```bash
code
```
5. 也可以在全部软件界面找到vscode
![vscodeicon](https://raw.githubusercontent.com/muzilyd/blog-image/main/jetson%20agx%20xavier/vscodeicon.png)

### 八 Jetson agx xavier qq安装
1. 首先进入[qq官网链接](https://im.qq.com/linuxqq/download.html)下载安装包
![qqdownload](https://raw.githubusercontent.com/muzilyd/blog-image/main/jetson%20agx%20xavier/qqdownload.png)
2. 当前版本的QQ Linux版依赖gtk2.0，安装QQ Linux版前请确保你的系统已安装gtk2.0，根据以下命令安装：
```bash
sudo apt install libgtk2.0-0
```
3. 开始安装qq
```bash
sudo dpkg -i linuxqq_2.0.0-b2-1089_arm64.deb
```
4. 如果版本更新后登录出现闪退情况，请删除 ~/.config/tencent-qq/#你的QQ号# 目录后重新登录
5. 然后你就能看到你的qq啦!注意linux上的qq只能用二维码登录哦，不要要求太多啦，能传文件聊天就行，嘿嘿嘿~

### Jetson agx xavier全套安装到此结束，各位小伙伴觉得不错的话可以给我哦
<a href="/merger/">打赏</a>
