---
layout: post
title:  "win10手动添加程序到右键菜单"
description: 搬砖画了一些图
date: 2021-04-06 14:08:55
---

# win10手动添加程序到右键菜单
装PyCharm的时候忘了配置在右键菜单中添加`用PyCharm打开文件夹`的快捷方式，学习了一下，顺便把VS code也配置上了。

1、打开注册表，找到`HKEY_CLASSES_ROOT\Directory\Background\shell`。

2、选择shell，右键，新建项，自定义名字，如PyCharm。

3、选中刚才新建的项，右键新建项，输入名字为 `command` 。这个名字不能改。

4、command里数据字段设置为要加入的那个程序的exe文件路径，或者是应用程序的指令，比如用PyCharm打开文件夹即为`"Path/of/PyCharm.exe" "%V"`。VS code打开文件夹也是`"路径""空格""%V"`，打开文件则是`"路径""空格""%1"`。不加空格也没问题。

5、然后可以添加选项的图标，右键`PyCharm`项，新建`字符串值`，名称改为`Icon`，路径是程序图标的地址。或者新建`可扩充字符串值`，直接使用exe路径即可。

6、可以任意自定义右键菜单了~