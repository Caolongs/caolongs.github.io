---
layout: post
title: "XtraFinder"
date: 2018-04-21 
description: "------"
tag: 工具
--- 

# XtraFinder

https://www.trankynam.com/xtrafinder/

在早期的OS X上，只需打开XtraFinderInstaller即可安装XtraFinder。

从OS X 10.11开始，系统完整性保护会阻止代码注入（以及其他许多事情）。
XtraFinder的工作原理是将其代码注入Finder应用程序进程。
为了安装XtraFinder，您需要禁用系统完整性保护。
XtraFinder安装完成后，您可以重新启用系统完整性保护。

有关系统完整性保护的更多信息，请访问此页[XtraFinder 系统完整性保护(SIP)](https://caolongs.github.io/2018/04/XtraFinder-%E7%B3%BB%E7%BB%9F%E5%AE%8C%E6%95%B4%E6%80%A7%E4%BF%9D%E6%8A%A4(SIP)/)


### 1、第一次安装XtraFinder的步骤
1.禁用系统完整性保护。
2.打开XtraFinderInstaller安装XtraFinder。
3.重新启用系统完整性保护。


### 2、更新XtraFinder
您不需要重复安装过程。
只需将XtraFinder复制到/ Applications目录即可。


### 3、无需打开XtraFinderInstaller即可手动安装
1.将`Extra`目录中的`XtraFinderInjector.osax`复制到`/System/Library/ScriptingAdditions`
2.将`XtraFinder`复制到`/Applications`


### 4、禁用系统完整性保护的步骤
1.通过重新启动计算机并在启动时按住`Command和R`键启动到恢复操作系统。
2.从 Utilities 菜单启动 Terminal。
3.输入以下命令：`csrutil disable`
4.重新启动电脑。

### 5、将系统完整性保护恢复到原始状态：
启动到恢复操作系统并输入以下命令：`csrutil clear`

