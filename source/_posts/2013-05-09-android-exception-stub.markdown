---
layout: post
title: "Exception:java.lang.RuntimeException: Stub"
date: 2013-05-09 17:50
comments: true
categories: Exception  
---

在实现一个通用方法:将任意可序列对象转换成成字符串,发生一个异常。  
 
```  
Exception in thread "main" java.lang.RuntimeException: Stub!
	at android.util.Base64.encodeToString(Base64.java:8)
	at com.example.android_sample.util.StringUtil.convertObj2String(StringUtil.java:30)
	at com.example.android_sample.util.StringUtil.main(StringUtil.java:63)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)
	at java.lang.reflect.Method.invoke(Method.java:597)
	at com.intellij.rt.execution.application.AppMain.main(AppMain.java:120)
```  
原因:  
Android工程里写了一个Java类，企图使用main方法来运行；但是主类中导入的是android的jar包：  
``` 
import android.util.Base64;
``` 
一般来说出现这个异常的原因是：  
一个地方调用了不属于这个地方的库。比如写java程序，但是我导入了android的相关包，调用android相关包时候会出发这个异常.