title: about hexo
date: 2013-08-18 17:10:48
tags: hexo
thumbnailImagePosition: right
autoThumbnailImage: yes
thumbnailImage: http://www.dev-metal.com/wp-content/uploads/2013/11/Octocat-800x605.png
gallery:
    - http://image.lxway.com/upload/3/22/322ecfea11e750a7caadf37c3842e283_thumb.png "hexo-github"
---
从Octopress 换到Hexo，确实速度提升不少。 

#### 关于Hexo
http://zespia.tw/hexo/
<!--more--> 
#### 遇到的几个问题： 
#### Issues 
[Issue List](https://github.com/tommy351/hexo/issues?direction=asc&sort=long-running&state=closed "issues")  

- hexo 命令不起作用 
```
language: zh-CN, 属性名称冒号后面必须要有一个空格，否则也不报错... 
```
- deploy struck. 
```
换成SSH path可以解决了。 
HTTP:https://github.com/<username>/<repo_name>.git 
SSH: git@github.com:<username>/<repo_name>.git 
```
- deploy fatal.
```
fatoal:Not a git repository <or any of the parent directories>: .git
>>要加缩进，Tab
deploy:
    type: github
    repository: https://github.com/zhuyong/log.git 
    branch: gh-pages
```
####autoThumbnailImage Test
![](http://image.lxway.com/upload/3/22/322ecfea11e750a7caadf37c3842e283_thumb.png)

####Gallery Test

