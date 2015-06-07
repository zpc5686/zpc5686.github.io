---
layout: post
title: "Notes of Stanford iOS dev"
date: 2015-01-20 22:31:37 +0800
comments: true
categories: 
---

以前在S的时候做过iOS的开发，但那时候总感觉自己学的不够扎实，基本上都是靠着githbub、stackoverflow以及apple的官方文档过来的，没有对iOS开发有一个系统对了解。

##第一讲
课程刚开始是一些闲话，需要一些前导课等等。后面提到一个APP分为4层，从下往上依次是
* core os 一个unix内核，socket等等都封装在这一层
* core services 数组、字典、GPS、多线程等等
* media 视频图像处理等
* Cocoa touch 各个控件，是高级对象

###MVC 设计模式
MVC是iOS应用的设计模式
* model与view之间永远不能通信
* view与controller之间通信的时候用的target action方法，我们在手机上碰到某些控件的时候会触发一些事件。这些事件有will、should、did这三种响应

###setter and getter
objective-c 将实例变量封装成了一个property。oc中所有对象都存于堆中，在栈上不能分配对象。
```@property (strong) NSString *content```
调用的时候card.content
strong （reference countting） 只要有strong指针指向它时，就不会被销毁。
weak 当没有strong pointer 指向它时 weak pointer变成nil，而且会销毁那块内存。

nonatomic 线程不安全。getter和setter可以重命名。

###学生提问
* 所有的对象都要被指针指着，
* 可以用[ ]来调用getter和setter，但是不推荐
* 除了getter和setter之外的方法不能用打点调用

##第二讲

1、上来就写了一个新类 Deck。
2、向NSMutableArray里插入nil后会使程序crash掉。
3、property可以用来给getter重新起名字比如
@property (nonatomic,getter=isMatched) BOOL matched;
在这里使用getter的时候用self.isMatched，而使用setter的时候还是要永self.mathced;
4、


