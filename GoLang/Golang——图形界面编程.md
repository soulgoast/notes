# Golang——图形界面编程

GUI:图形界面编程，方便非专业用户使用

## GTK简介

GTK（GIMP Toolkit）是一套在GIMP的基础上发展而来的高级的，可伸缩化、夸平台图形工具包，提供一整套完备的图形构件，适用于大大小小各种软件工程项目，不论是小到只需要一个窗口，还是复杂得如桌面系统。

简单来说，GTK是一种用来帮助制作图形交互界面的函数库。



## 搭建Go版本GTK（Windows）开发环境

参考文档：

https://blog.csdn.net/tennysonsky/article/details/79221507

```cmd
## 问题
C:\Users\m1896>pkg-config --cflags gtk+-2.0
Package gtk+-2.0 was not found in the pkg-config search path.
Perhaps you should add the directory containing `gtk+-2.0.pc'
to the PKG_CONFIG_PATH environment variable
No package 'gtk+-2.0' found
## 解决办法


```

