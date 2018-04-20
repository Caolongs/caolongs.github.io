---
layout: post
title: "MWeb + Jekyll"
date: 2018-04-20 
description: "------"
tag: 工具
---   

# MWeb + Jekyll

### 1. 打开 Mweb 外部模式

若经常使用外部模式，可在偏好设置中设置：
`cmd+,` 选中`通用设置`,勾选`启用时默认打开外部模式`

![](/images/media/15241911732330.jpg){:height="50%" width="70%"}


 
  


### 2. 添加引用文件
点击侧边栏左下角`+`,引入 github.io 项目文件 
![](/images/media/15241912074172.jpg){:height="50%" width="70%"}


### 3. 设置引用文件

Jekyll 等静态博客因为可以自定像 http://域名/2015/3/the-blog-post/ 这样的网址，所以在增加图片时，都是用 /images/pic.jpg 这样的绝对路径。然后图片要放在 source/images 文件夹下。
总之，最重要一点请选择 **绝对位置**

![](/images/media/15241912003700.jpg){:height="50%" width="70%"}




### 4. 编辑文件
Jekyll 一般是在`_posts`文件加下新增编辑发布文档，选中所在目录，添加文档就可，实时同步本地github文件

### 5. 发布文件
完成需要编辑的文档可运行 jekyll 服务器

```
$ jekyll server
```
![](/images/media/15241912904031.jpg){:height="50%" width="70%"}

cd 到github.io当前目录下，执行git提交命令即刻完成上传

更多知识待研究，如发布脚本。。。

## 其他
更多学习请参照  [MWeb for Mac 帮助](http://zh.mweb.im/help.html)


