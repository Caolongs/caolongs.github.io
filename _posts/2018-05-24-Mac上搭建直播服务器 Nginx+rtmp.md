---
layout: post
title: "Mac上搭建直播服务器 Nginx+rtmp"
date: 2018-05-24 
description: "------"
tag: 音视频
---


## 简介
[Nginx](https://zh.wikipedia.org/wiki/Nginx) 是非常优秀的开源服务器，用它来做hls或者rtmp流媒体服务器是非常不错的选择，

## 1、安装
增加对 nginx 的扩展;也就是从github上下载,home-brew对ngixn的扩展

执行克隆命令,github的项目(https://github.com/denji/homebrew-nginx)

```
$ brew tap denji/nginx 
```

**注意:** `brew tap homebrew/nginx` 报下面的错误, 使用`brew tap denji/nginx `替代
homebrew/nginx was deprecated. This tap is now empty as all its formulae were migrated.


## 2、执行安装命令:

```
$ brew install nginx-full --with-rtmp-module 
```
 


### 查看 nginx 安装在哪里

```
$ brew info nginx-full
```

```
--HEAD
	Install HEAD version
==> Caveats
Docroot is: /usr/local/var/www

The default port has been set in /usr/local/etc/nginx/nginx.conf to 8080 so that
nginx can run without sudo.

nginx will load all files in /usr/local/etc/nginx/servers/.

- Tips -
Run port 80:
 $ sudo chown root:wheel /usr/local/opt/nginx-full/bin/nginx
 $ sudo chmod u+s /usr/local/opt/nginx-full/bin/nginx
Reload config:
 $ nginx -s reload  ## 重新加载配置文件
Reopen Logfile:
 $ nginx -s reopen  ## 再次打开配置文件
Stop process:
 $ nginx -s stop    ## 停止服务器
Waiting on exit process
 $ nginx -s quit    ## 退出服务器

To have launchd start denji/nginx/nginx-full now and restart at login:
  brew services start denji/nginx/nginx-full
Or, if you don't want/need a background service you can just run:
  nginx
```

* nginx安装所在位置  `/usr/local/opt/nginx-full/bin/nginx`
* nginx配置文件所在位置  `/usr/local/etc/nginx/nginx.conf`
* nginx服务器根目录所在位置  `/usr/local/var/www`


### 启动nginx服务

```
$ nginx
```
在浏览器地址栏输入：http://localhost:8080
出现Welcome to nginx ,代表nginx安装成功了。

## 3、配置rtmp

打开配置文件 `/usr/local/etc/nginx/nginx.conf`

```
http {
    ……
}

#在http节点下面(也就是文件的尾部)加上rtmp配置：
rtmp {
    server {
        listen 1935;
        application abcs {
            live on;
            record off;
        }
    }
}
```

* rtmp 是协议名称
* server 说明内部中是服务器相关配置
* listen 监听的端口号, rtmp协议的默认端口号是1935
* application 访问的应用路径是 abcs
* live on; 开启实时
* record off; 不记录数据


## 4、 保存文件后，重新加载nginx的配置文件

```
$ nginx -s reload
```


## 5、安装ffmepg工具

```
$ brew install ffmpeg
```
安装这个需要等一段时间, 这时你可以准备一个视频文件作为来推流，然后安装一个支持rtmp协议的视频播放器.Mac下可以用 VLC


## 6、通过ffmepg命令进行推流

```
ffmpeg -re -i [你的视频文件的绝对路径] -vcodec copy -f flv rtmp://localhost:1935/abcs/room

// 如：ffmpeg -re -i /Users/caolongjian/Desktop/CCVideo.mp4  -vcodec copy -f flv rtmp://localhost:1935/abcs/room
```

这里abcs是上面的配置文件中,配置的应用的路径名称;后面的room可以随便写。

## 7、 验证视频
然后电脑上打开vlc这个播放器软件 点击File---->Open Network 在弹出来的框中选择Network然后输入URL:

```
rtmp://localhost:1935/abcs/room
```


## 参考
[Mac上搭建直播服务器Nginx+rtmp](http://www.cnblogs.com/jys509/p/5649066.html)
[mac搭建naginx+rtmp服务器-带你出坑](https://blog.csdn.net/zcvbnh/article/details/79495285)

-----
[toc]

