---
layout: post
title: "Mac上搭建直播服务器 Nginx+HLS"
date: 2018-05-24 
description: "------"
tag: 音视频
---

HLS([HTTP Live Streaming](https://zh.wikipedia.org/wiki/HTTP_Live_Streaming))是一个由苹果公司提出的基于HTTP的流媒体网络传输协议。是苹果公司QuickTime X和iPhone软件系统的一部分。它的工作原理是把整个流分成一个个小的基于HTTP的文件来下载，每次只下载一些。当媒体流正在播放时，客户端可以选择从许多不同的备用源中以不同的速率下载同样的资源，允许流媒体会话适应不同的数据速率。在开始一个流媒体会话时，客户端会下载一个包含元数据的extended M3U (m3u8) playlist文件，用于寻找可用的媒体流。

Mac 直播服务器 Nginx+rtmp 见上文[Mac上搭建直播服务器 Nginx+rtmp](https://www.jianshu.com/p/cf74a34af15d)

下面需要对Nginx服务器增加对HLS的支持。在Nginx增加对HLS支持,修改下配置文件nginx.conf

## 找到http-->server,在花括号中增加


```
server {
        listen       8080;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }


        #HLS配置开始,这个配置为了`客户端`能够以http协议获取HLS的拉流
        location /hls {
            # Serve HLS fragments
            types {
                application/vnd.apple.mpegurl m3u8;
                video/mp2t ts;
            }
            root html;
            add_header Cache-Control no-cache;
        }
        #HLS配置结束


        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

    }
```


## 找到rtmp下的server在花括号中增加

```
#在http节点下面(也就是文件的尾部)加上rtmp配置：
rtmp {
    server {
        listen 1935;
        application zbcs {
                live on;
                record off;
            }
        #增加对HLS支持开始
        application hls {
            live on;
            hls on;
            hls_path /usr/local/var/www/hls;
            hls_fragment 5s; 
        }
        #增加对HLS支持结束
    }
}
```

* live on; 开启实时
* hls on; 开启hls
* hls_path; ts文件存放路径
* hls_fragment 5s; 每个TS文件包含5秒的视频内容


## 保存配置文件，重新加载nginx配置

```
$ nginx -s reload
```

## 通过ffmepg命令进行推流

ffmpeg推流还是和上一篇的一样，不过，我们需要推到新配置的hls中，room 关键字可以任何替换

```
$ ffmpeg -re -i /Users/caolongjian/Desktop/CCVideo.mp4  -vcodec copy -f flv rtmp://localhost:1935/hls/room
```

然后，我们在就可以在这个目录下（这个也是Nginx下html默认配置文件）`/usr/local/var/www/hls`看到生成一个个ts的文件，还会生成一个”你的m3u8的文件名称.m3u8“的文件


## 测试拉流

通过上面的配置，我们可以同时通过rtmp和hls两种播放方式来看到推出来的流。注意，如果使用 http 方式，则是监听的 8080 端口，这个是在配置文件里写的

(1) 用rtmp （使用VLC验证播放）

```
rtmp://localhost/hls/movie
```

(2) 用hls（播放使用VLC验证播放）

```
http://localhost:8080/hls/room.m3u8
```

(3)我们还可以在Safari浏览器里输入上面的地址直接播放`http://localhost:8080/hls/room.m3u8`



## 参考

[Mac直播服务器Nginx配置对HLS的支持](http://www.cnblogs.com/jys509/p/5653720.html)


------
[toc]

