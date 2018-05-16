---
layout: post
title: "XtraFinder 系统完整性保护(SIP)"
date: 2018-04-21 
description: "------"
tag: 工具
--- 

 
[XtraFinder 系统完整性保护原文](http://www.trankynam.com/xtrafinder/sip.html)（译）

### 1、关于OS X 10.11中的系统完整性保护

[苹果的文章](https://support.apple.com/en-us/HT204899)。
系统完整性保护阻止代码注入（以及其他许多事情）。

XtraFinder的工作原理是将其代码注入Finder应用程序进程。


### 2、如何让XtraFinder在OS X 10.11中工作
您需要部分禁用系统完整性保护。

我不鼓励您禁用系统完整性保护。它会让你的电脑不安全。

### 3、如何部分禁用系统完整性保护
参考这篇[苹果的文章](https://developer.apple.com/library/content/documentation/Security/Conceptual/System_Integrity_Protection_Guide/ConfiguringSystemIntegrityProtection/ConfiguringSystemIntegrityProtection.html)。

按着这些次序：

##### 1. 通过重新启动计算机并在启动时按住`Command+R`键启动到恢复操作系统。
##### 2. 从 Utilities 菜单启动 Terminal。
##### 3. 输入以下命令：`csrutil enable --without debug`
##### 4. 重新启动您的计算机。

### 4、“csrutil enable --without debug” 命令的作用是什么？
它允许代码注入。这意味着XtraFinder可以将其代码注入Finder应用程序进程。


### 5、如何将系统完整性保护恢复到原始状态
启动到恢复操作系统并输入以下命令：`csrutil clear`

完全禁用SIP `csrutil disable`
启用SIP `csrutil enable`

