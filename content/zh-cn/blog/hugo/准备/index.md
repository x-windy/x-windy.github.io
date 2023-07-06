---
date: 2023-07-06T16:40:29+08:00
title: "准备"
linkTitle: "准备"
description: ""
author: "XW"
resources:
  - src: "**.{png,jpg}"
    title: "Image #:counter"
    params:
      byline: "Photo: 你好"
---

> 系统环境：Windows 11 64位

1.下载Hugo：[Hugo Releases ](https://github.com/gohugoio/hugo/releases)，并解压

2.添加环境变量

>- 在`设置-系统-系统信息-高级系统设置-环境变量-系统变量-Path`添加解压的文件夹目录地址
>
>- 或直接搜索`编辑系统环境变量` ，在 `环境变量-系统变量-Path`添加解压的文件夹目录地址

3.`Win+R`打开运行输入`cmd`打开命令指令行，输入

```
hugo version
```

检查全局变量是否生效，返回以下内容说明hugo安装成功

```shell
hugo v0.115.1-857374e69358f788bd31ddc55255c5c8e3dcfd80+extended windows/amd64 BuildDate=2023-07-03T17:28:25Z VendorInfo=gohugoio
```

4.使用hugo新建一个博客，在命令行输入

```shell
# 两种配置文件格式
hugo new site blog			# toml格式
hugo new site blog -f yml	# yml格式
# 生成的目录结构
├─assets			资源目录
├─content			内容目录(存放.md文件) 就是要发布的文章
├─data				数据目录
├─layouts			网站模版目录
├─public			存放生成的站点文件
├─static			静态文件目录(JS、CSS、图片)
├─themes			主题目录
├─archetypes		(存放.md文件的模板) 就是生成文章时用到前言模板
│      default.md  	内容模版
└─config.toml		配置文件
```

5.启动服务器

```shell
cd blog
hugo server
```

在浏览器输入`http://localhost:1313/`打开网站

显示`Page Not Found`说明网站搭建成功

6.安装主题

[Hugo Themes](https://themes.gohugo.io/)

使用的主题模板：[docsy](https://github.com/google/docsy)

用`git`将项目克隆到`themes`文件夹里面

[docsy-example](https://github.com/google/docsy-example)











## Including images

Here's an image (`hugo.jpg`) that includes a byline and a caption.

{{< imgproc hugo Fill "600x300" >}}
Fetch and scale an image in the upcoming Hugo 0.43.
{{< /imgproc >}}

The front matter of this post specifies properties to be assigned to all image resources:

```
resources:
- src: "**.{png,jpg}"
  title: "Image #:counter"
  params:
    byline: "Photo: Riona MacNamara / CC-BY-CA"
```

To include the image in a page, specify its details like this:

```
{{< imgproc hugo Fill "600x300" >}}
Fetch and scale an image in the upcoming Hugo 0.43.
{{< /imgproc >}}
```

The image will be rendered at the size and byline specified in the front matter.