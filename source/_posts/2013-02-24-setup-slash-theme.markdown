---
layout: post
title: "为octopress安装slash主题"
date: 2013-02-24 20:10
comments: true
categories: octopress
---
从github上获取主题文件,然后安装,如下:  
```
cd octopress
git clone git@github.com:tommy351/Octopress-Theme-Slash.git .themes/slash
rake install['slash']
rake generate
```
为便于以后自己从github上获取博客更新，将主题文件push到github库上。
