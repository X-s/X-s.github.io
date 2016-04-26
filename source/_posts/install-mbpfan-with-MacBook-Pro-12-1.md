---
title: '在MacBook Pro 12-1 with ubuntu 15.10 上安装风扇控制器（mbpfan）'
date: 2016-02-01 09:25:57
tags: [MacBook, ubuntu]
---
## 前言

  最近入了一款MacBook Pro 12-1（R屏 2015年 13款），由于工作内容需求，不得不安装了一份ubuntu 15.10，在使用过程中发现风扇转速不能很好的控制，经常出现温度过高，风扇却没怎么转的情况，Google了一下，发现有一个东西叫做`mbpfan`，这里记录一下安装及使用过程。
<!-- more -->

*本文所有内容都基于ubuntu 15.10操作而来，不保证其他OS同样适用*

### 开始安装

- 首先运行`git clone https://github.com/dgraziotin/mbpfan.git`将`mbpfan`源码下载下来，当然你也可以选择zip download等方法

- 将传感器mod加载进内核

```sh
xs@xs-MacBookPro:~$ sudo vim /etc/modules
#add coretemp and applesmc to save
#修改后的文件内容，已经加入coretemp和applesmc
xs@xs-MacBookPro:~$ cat /etc/modules
# /etc/modules: kernel modules to load at boot time.
#
# This file contains the names of kernel modules that should be loaded
# at boot time, one per line. Lines beginning with "#" are ignored.
coretemp
applesmc
xs@xs-MacBookPro:~$
```

- 进入`mbpfan`源码目录开始进行安装测试

```sh
cd mbpfan #确保终端当前路径跟刚才clone所处路径相同
sudo apt-get install build-essential #安装编译必需环境
make
sudo make install #此命令会将mbpfan安装到/usr/sbin，将配置文件安装到/etc/mpfan.conf
sudo make tests #测试命令，会显示本机风扇、内核等关键信息
sudo service mbpfan start
```

- 现在`mbpfan`已经可以正常运行了，但我们还要将其加入开机启动

```sh
sudo cp mbpfan.service /etc/systemd/system/ #systemd start
sudo systemctl daemon-reload
sudo systemctl start mbpfan.service
sudo systemctl enable mbpfan.service
sudo systemctl start mbpfan #此时mbpfan就已经加入开机启动了，会每次跟随系统启动
```

- 关于`mbpfan`配置文件，大家可以自行更改，我将默认的转速降低到1000，低温感受不到风扇声音了

```sh
xs@xs-MacBookPro:~$ cat /etc/mbpfan.conf
[general]
min_fan_speed = 1000	# default is 2000
max_fan_speed = 6200	# default is 6200
low_temp = 63			# try ranges 55-63, default is 63
high_temp = 66			# try ranges 58-66, default is 66
max_temp = 86			# do not set it > 90, default is 86
polling_interval = 7	# default is 7
xs@xs-MacBookPro:~$
```

{% asset_img fan_speed.jpg %}

*参考文献*

> https://github.com/dgraziotin/mbpfan
