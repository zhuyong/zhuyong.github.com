---
layout: post
title: "在github上搭建octopress"
date: 2013-02-23 09:51
comments: true
categories: octopress
---
今天突然想到上学时老师常教诲我们:好记性不如烂笔头。再则，年纪大了，记性差，不服老不行啊。
本文目的：  
一、记录一下自己搭建的过程。  
二、方便其他需要搭建的朋友，毕竟放狗搜也需要花时间啊，如果有一篇比较完整的说明，那岂不乐哉~，但本文并不可能涵盖所有问题，所有不足，敬请留言。

####主要内容：  
- 准备工作  
- 安装Git    
- 安装Ruby  
- 安装octopress及配置  
- 写博客及发布步骤  
- 故障排除
<!--More-->
##### 一、准备工作
1.下载Git,smartGit(可选)  
2.下载RubyInstaller(版本1.9.3)和DevKit。

##### 二、安装Git
Git安装(略)  
(1) 生成SSH key,用于push代码到github及从github上pull代码,运行git bash,并执行命令：
```
ssh-keygen -C "username@email.com" -t rsa

Note: “username@email.com”需要更换成你在Github上注册的Email地址或者是Username
```

(2) 将生成的key添加至github上

(3) 测试一下连接，如失败，请参考[故障排查-无法连接github.com故障]
```
$ ssh -v git@github.com
```

##### 三、安装ruby
（1） 安装ruby,按提示步骤做就行了(略)  
（2） 在“Start Command Prompt with Ruby”命令行中进入DevKit解压缩的目录，然后运行以下命令:
```
ruby dk.rb init
ruby dk.rb install
gem install rdiscount --platform=ruby
```

##### 四、安装Octopress及配置

（1）获取octopress源码，通过Git从Github上克隆一份Octopress（在Git Bash上输入命令）  
```
git clone git://github.com/imathis/octopress.git octopress  
```
（2）安装一些依赖的工具（后面都是在Start Command Prompt with Ruby中输入）  
```
cd octopress  
ruby --version # Should report Ruby 1.9.3  
gem install bundler  
bundle install   

Note:bundle install 需要翻墙,或者更新源，如何更新源，请参考[更新ruby gem源安装bundle]
```

(3) octopress配置  
```  
	配置_config.yml. (略，因为地球人都知道，注意点:冒号后必须留有一个空格) 

	编码设定:    
到Ruby的安装目次\lib\ruby\gems\1.9.1\gems\jekyll-0.11.2\lib\jekyll\找到convertible.rb这个文件，  
将self.content = File.read（File.join（base， name））  
批改为  
self.content = File.read（File.join（base， name）， :encoding => "utf-8"）。
```

##### 五、写博客及发布基本步骤
（1）创建一篇新的博文，两种方式:  
```
1. 复制已创建文件或模板进行修改  
2. 使用rake命令创建新的文件:  
	rake new_post["fileName"]
```
（2）将markdown文件转换成html并预览
```
rake generate
rake preview  
预览地址:http://localhost:4000
```

（3）设置发布的github库，进入Octopress目录,执行命令:  
```
rake setup_github_pages  
按提示信息给事输入你的github SSH地址:
git@github:yourname/yourname.github.com
```

（4）发布博文到github
```
rake deploy
```

（5）将source文件添加到github库  
```
git status  
git add .  
git commit -m 'your message'  
git push origin source  
```



##### 六、故障排查
1. 乱码问题:  
	(1) smartGit乱码问题.post保存为UTF-8 without BOM

2. 无法连接github.com故障
```  
$ ssh -v git@github.com 连接故障  
如果发生下面的错误：   
OpenSSH_4.6p1, OpenSSL 0.9.8e 23 Feb 2007  
debug1: Connecting to github.com [207.97.227.239] port 22.  
debug1: connect to address 207.97.227.239 port 22: Attempt to connect timed out  
without establishing a connection  
ssh: connect to host github.com port 22: Bad file number    

解决办法：
原因：22端口未开放问题解决,增加文件  
.ssh\config  
Host github.com  
User your_name  
Hostname ssh.github.com  
PreferredAuthentications publickey  
IdentityFile ~/.ssh/id_rsa  
port 443    
```
3. Gem源导致故障  
```
安装rdiscount时可能会出现如下错误：  
C:\blog_github\DevKit>gem install rdiscount --platform=ruby  
ERROR:  Could not find a valid gem 'rdiscount' (>= 0) in any repository  
ERROR:  Possible alternatives: rdiscount  

解决办法：设置gem源
删除原有gem source  
gem source -r http://rubygems.org/  
gem source -r http://production.s3.rubygems.org/   

增加新source源  
gem source -a  http://production.s3.ru

查看现有的gem源：  
gem sources -l

增加淘宝源  
gem source -a http://ruby.taobao.org/

修改源后，再执行一下：  
gem install rdiscount --platform=ruby
```

4.bundle install 需要翻墙或则更改源
```
参考：http://www.iteye.com/news/23813-rubygems-taobao
如果是在Rails当中使用bundle，则需要修改Gemfile文件，将默认的 
修改octopress下的gemfile

source 'http://rubygems.org'  
改成  
source 'http://ruby.taobao.org/' 
```

#####TODO
- 完整的搭建过程（网上有很多，但是还是比较零散，搭建时也查了不少文章，遇到问题总是要放狗）
- 换一个主题.就slash吧，简洁明了,
- 加tag,
- 加评论等...
<!--More-->