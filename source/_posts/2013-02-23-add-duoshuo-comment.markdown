---
layout: post
title: "为octopress增加多说评论"
date: 2013-02-23 14:30
comments: true
categories: octopress
---
1.注册多说账户,并配置站点信息.   
2.修改配置文件:_config.yml,是否显示多说评论模块，代码如下：  
```
  #Duoshuo Comments  
   duoshuo_show_comment_count: true  
   duoshuo_short_name: your_duoshuo_username
```
3.修改source/_layouts/page.html  
在disqus模块代码下追加duoshuo模块，代码如下：
![code in page.html](/images/myImages/duoshuo_code.PNG)
<!--More-->
4.修改source/\_layouts/post.html中也做如上的修改   
5.按路径创建文件:source/\_includes/post/duoshuo.html,代码如下（也就是多说网，配置站点后生成的代码）    
注意:short_name已用变量替换：  
```
<!-- Duoshuo Comment BEGIN -->  
	<div class="ds-thread"></div>  
	<script type="text/javascript">  
	var duoshuoQuery = {short_name:"{{ your_duoshuo_name }}"};
	(function() {  
		var ds = document.createElement('script');  
		ds.type = 'text/javascript';ds.async = true;  
		ds.src = 'http://static.duoshuo.com/embed.js';  
		ds.charset = 'UTF-8';  
		(document.getElementsByTagName('head')[0]
		|| document.getElementsByTagName('body')[0]).appendChild(ds);
	})();  
	</script>  
<!-- Duoshuo Comment END -->
```

6.修改 _includes/article.html 文件，在末尾disqus评论代码下添加duoshuo评论,代码如下：  
![code in article.html](/images/myImages/duoshuo_code2.PNG)

OK,收工。