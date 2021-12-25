---
redirect_from: /_posts/2021-12-31-Jetson agx xavier刷机+pcle固态加装+pytorch+miniforge(anaconda)+vscode+wifi+ros全套安装(亲测有效，完美通过无需踩坑).md/
title: Jetson agx xavier刷机+pcle固态加装+pytorch+miniforge(anaconda)+vscode+wifi+ros全套安装(亲测有效，完美通过无需踩坑)
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
<p/><br/>(2) 50%左右（各个人不一样）agx自动开机，需要重新进行配置，然后记住你设置的账号密码地址，后面虚拟机上需要输入（如果agx地址没有自动显示可以再agx终端上使用ifconfig命令查看）</br></p>
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

### 二、Jetson agx xavier pcie固态加装
1. 买一块M.2KeySSD的固态
2. 将agx倒过来把两个支架拆开
3. 用卡片或者小针伸进板子和壳之间的缝里撬开它，他们之间主要就是依靠下图的这些卡口连在了一起所以直接撬开就行，**注意撬开后一定要慢慢拿起**，里面有一根风机线很细容易断
![agxpcie](https://raw.githubusercontent.com/muzilyd/blog-image/main/jetson%20agx%20xavier/agxpcie.png)
4. 将买的固态插槽内
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

### 四、Jetson agx xavier pytorch安装
