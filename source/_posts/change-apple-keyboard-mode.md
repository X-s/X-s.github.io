---
title: '在安装Ubuntu的MacBook上更改Apple 键盘模式'
date: 2016-04-23 22:25:57
tags: [MacBook, ubuntu]
---
  当你的`MacBook`处于`ubuntu`系统下时，可能会发现无法打出`~`按键的情况，或者想恢复F1-F12
为普通按键，而不是功能键（毕竟ubunut系统下，更多使用的是普通的按键），所以这里记录一下修改方法

<!-- more -->

*很简单的修改方法，直接给出操作步骤*

```bash
sudo su -
#更改FN功能键模式为普通模式
echo "options hid-apple fnmode=2" > /etc/modprobe.d/hid-apple.conf
#更改键盘布局(for proper ~`)
echo "options hid-apple iso_layout=0" > /etc/modprobe.d/hid-apple.conf
reboot
```

>*参考资料*
>https://help.ubuntu.com/community/AppleKeyboard
