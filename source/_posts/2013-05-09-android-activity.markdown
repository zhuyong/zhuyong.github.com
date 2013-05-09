---
layout: post
title: "学习笔记:Android组件之Activity"
date: 2013-05-09 17:30
comments: true
categories: reading Android  
---
电子书:[Android开发权威指南].李宁.扫描版  

####主要内容：  
- 创建Activity  
- 配置Activity    
- 启动Activity  
- Activity生命周期  
- Activity间数据传递  
- 返回数据给前一个Activity
<!--More-->
##### 一、创建Activity
1. 继承了android.app.Activity类的Java类.   

##### 二、配置Activity  
要使用Activity必须在Mainifest中声明,格式如下:  
```
<activity android:name=".MainActivity"
    android:label="@string/app_name">
        <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
         </intent-filter>
</activity>  
```
andorid:name属性定义为类名，有如下几种方式:  
1）只定义类名，但是会引用<Application> 里的package属性。  
2）定义完整的类名，即：包名+类名  
3）定义相对类名，即：既相对于<Application>里package 的类名。    

##### 三、启动Activity  
（1） 显示调用Activity  
```
Intent intent = new Intent(this,YourActivity.class);  
或者
Intent intent = new Intent();  
intent.setClass(this,YourActivity.class);  
```
（2） 隐式调用Activity  
隐式调用仍然需要使用Intent，但是不需要指定需要调用的Activity,而只要指定一个Action和相应的Category。  
```
To be continue... 
```

##### 四、Activity生命周期  

（1）启动应用程序: onCreate->onStart->onResume    
（2）失去焦点: onPause->onStop  
（3）重新获得焦点: onRestart->onStart->onResume      
（4）退出应用程序: onPause->onStop->onDestory      

##### 五、Activity之间传递数据
（1）使用Intent对象进行数据传递，但是Intent无法传递不能序列化的对象。  
如果自定义类的对象，必须实现java.io.Serializable接口。 
（2）使用静态变量进行数据传递  
在切换Activity之前对静态变量进行赋值，即在startActivity之前赋值，Activity跳转后，仍然可以取到。    
（3）使用剪切板传递数据  
```
ClipboardManager clipboardManager = 
(ClipboardManager)getSystemService(Context.CLIPBOARD_SERVICE);
```
通过剪贴板保存复杂的数据  
将可序列对象转换为byte[]类型的数据，然后将byte[]类型的数据再通过base64进行编码转换成字符串。  
然后就可以传输了。  
TODO:实现一个通用方法，将任意可序列对象转换成成字符串。  
    
（4）使用全局对象传递数据  
虽然静态变量可以用来传递任何类型的数据，但是如果在类中大量使用静态变量（尤其是使用很占资源的变量）可能会造成内存溢出，而且还会造成因为静态变量在多个类中出现造成代码维护混乱。  
使用全局对象传递数据的方式可以完全取代静态变量。  
全局对象所对应的类必须是android.app.Application的子类：  
     
注：全局类中不需要定义静态变量，只需要定义成员变量即可，而且全局类中必须要有一个无参数的构造函数。  

总结：  
1.向其他Activity传递简单类型（int、String、short、bool等）或可序列化的对象时，建议使用Intent.  
2.如果传递不可序列化的对象，可以采用静态变量或全局对象的方式。  

##### 六、返回数据到前一个Activity

```  
   Intent intent = new Intent(MyActivity.this,MyActivity4.class);
   startActivityForResult(intent,1);	// Request Code:1

   // 使用返回的数据
   protected void onActivityResult(int requestCode, int resultCode, Intent data) 
   { 
     switch (requestCode)
     {
        case 1:
           switch(resultCode)
           {
              case 2: 
              break;
            }
           break;
        ...

        Default:
           break;
      } 
    };
```  

```
   // 数据所在Activity,通过Intent进行传值
   Intent intent = new Intent();
   intent.putExtra("value",value);
   setResult(2,intent); // ResultCode:2
   finish();
```