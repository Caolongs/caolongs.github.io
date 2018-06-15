---
layout: post
title: "iOS逆向-砸壳（Clutch & dumpdecrypted）"
date: 2018-06-15 
description: "------"
tag: 逆向与安全
---


> MachO文件 -> 苹果加密 -> 加壳文件
>
> 加壳文件 -> 苹果解密 -> MachO文件（DYLD）

> 解密过程：
> 1. DRM（数字版权管理）检查
> 2. 找到合适结构进行解密
> 3. 使用DYLD加载解密的MachO

## 砸壳工具：
Clutch 静态砸壳
dumpdecrypted 动态加壳


静态砸壳：调用系统的解密程序
动态砸壳：得到系统解密后的MachO文件


## Clutch 静态砸壳 

前提条件：手机已越狱

### 1. 下载安装Clutch到手机

github下载 [Clutch](https://github.com/KJCracks/Clutch/releases) 并命名为 `Clutch`

### 2. 将该文件拷贝到设备（手机）中

```
scp ./build/Clutch root@<your.device.ip>:/usr/bin/Clutch
    
若有端口映射
scp -P 2222 ./build/Clutch root@localhost:/usr/bin/Clutch
```
    
### 3. Clutch 增加可执行权限

```
# chmod +x Clutch
```

### 4. 使用 

```
Clutch [OPTIONS]
-b --binary-dump     Only dump binary files from specified bundleID
-d --dump            Dump specified bundleID into .ipa file
-i --print-installed Print installed application
--clean              Clean /var/tmp/clutch directory
--version            Display version and exit
-? --help            Display this help and exit
```

* `$ Clutch i `列出可砸壳应用

    ```
    1: 微信 <com.tencent.xin>
    2: ....
    ```

* 砸壳

    ```
    # Clutch -d [标示或bundleID]
    
    如：
    Clutch -d 1
    ```
    成功后可看见砸壳后的文件


## dumpdecrypted 动态加壳

### 1. 下载并执行

github [dumpdecrypted](https://github.com/stefanesser/dumpdecrypted) `clone and Download`

当前文件夹下执行`make`

```
$ make
```
生成 `dumpdecrypted.dylib` 和 `dumpdecrypted.o` 文件


### 2. 将动态库`.dylib`从本地拷贝到手机上


```
scp dumpdecrypted.dylib root@<your.device.ip>:~/
    
若有端口映射
scp -P 2222 ./build/Clutch root@localhost:~/
```

### 3. 将动态库`.dylib`依附到要砸壳的app进程中

利用 `DYLD` 环境变量 DYLD_INSERT_LIBRARIES=

```
# DYLD_INSERT_LIBRARIES=dumpdecrypted.dylib [进程路径]

如：
DYLD_INSERT_LIBRARIES=dumpdecrypted.dylib /var/mobile/.../WeChat.app/WeChat
```
注：进程路径可通过 `ps -A` 查看

### 4. ipa包
执行之后 当前目录下 `ls` 得到 `WeChat.decrypted`可执行文件
其他文件可在`/var/mobile/.../WeChat.app`拷贝组合成砸壳后的ipa包






