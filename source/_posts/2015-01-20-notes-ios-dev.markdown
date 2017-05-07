---
layout: post
title: "Notes of iOS programming"
date: 2015-01-20 22:31:37 +0800
comments: true
categories: 
---

以前在SAP实习的的时候做过iOS的开发，但那时候感觉自己学的不够扎实，基本上都是靠着github、stackoverflow以及apple的官方文档过来的，
没有对iOS开发有一个系统对了解。最近跟着big nerd ranch的iOS programming过一遍，书上比较散，在这里我把认为重要的记一下吧，能记多少是多少。

###一个APP的构成
一个能在手机上运行的APP，我们写的代码仅仅是它运行代码中很少的一部分。还有很多服务是cocoa和iOS为我们做的。

一个APP分为4层，从下往上依次是

* core os 一个unix内核，socket等等都封装在这一层
* core services 数组、字典、GPS、多线程等等
* media 视频图像处理等
* Cocoa touch 各个控件，是高级对象

###MVC 设计模式
MVC是iOS应用的设计模式

* model与view之间永远不能通信
* view与controller之间通信的时候用的target action方法，我们在手机上碰到某些控件的时候会触发一些事件。

这些事件有will、should、did这三种响应。

现在又推出一种新的设计模式，mvvm模式，不过本书中并没有提到，关于mvvm最著名的iOS开源项目应该是ReactiveCocoa吧。
##Objective-C 的基本知识


所有的对象都要被指针指着。

### 类

没有public 与 private 的修饰符，public的变量在.h文件中写，private的变量在类extension中写。

###初始化方法
Objective-C 的一个类可以有很多种初始化方法，但是必须要有一个指定初始化方法，怎么指定呢，指定初始化方法的格式如下

``` 
	self = [super init];
	
	if(self)
	{
		// ivar init
	}
	return self

```
值得一提的是，BNR书中的类初始化方法是直接访问实例变量来进行初始化的。在写初始化方法的时候，我们需要记住命名规则要以init开头！


###属性
Objective-C 将实例变量封装成了一个property。oc中所有对象都存于堆中，在栈上不能分配对象。
可以用[ ]来调用getter和setter，但是不推荐。

```
@property (nonatomic, strong) NSString *content

```

首先要理解实例变量和属性不是一回事情。

实例变量英文是instant variable ，很多地方简写成ivar

属性的英文是property。

关于属性与实例变量的操作，具体要看runtime的源代码，这里就先不研究了。但是当程序既覆盖了一个属性的存方法，又覆盖了取方法时，编译器不会再为其自动创建实例变量，如果我们需要访问实例变量的话，需要自己创建。可以看出属性的本质其实就是编译器为我们自动创建了存取方法。

一个property可以有多个修饰符，下面来分别谈谈这些修饰符。

#### 内存管理特性
* strong （reference countting） 只要有strong指针指向它时，就不会被销毁。
* weak 当没有strong pointer 指向它时 weak pointer变成nil，而且会销毁那块内存。
* copy 通常情况下，当某个属性是指向其他对象的指针，而且该对象的类有可被修改的子类(NSmutableString)时，应该将该对象设置为copy。block也是copy。如果自己写的类要实现copy 需要实现NSCopy的protocol
* unsafe_unretained 没有arc之前是assign，是默认的内存管理特性

#### 多线程特性
nonatomic 线程不安全。但大多都选择atomic，atomic性能太差

#### 读写特性
readwrite、readonly（没有setter方法了）

property可以用来给getter重新起名字比如

@property (nonatomic,getter=isMatched) BOOL matched;

###视图

uiviewcontroller 的一个重要的属性是view。它负责控制view，大概代码是这样的

```
- (UIView *)view
{
  if (!_view) {
    [self loadView];
    [self viewDidLoad]; 
  }
}

```
当viewcontroller发现view是nil时，会调用loadview方法。

* viewWillAppear 在用户每次看到view的时候都会调用，与viewDidAppear相同（相应的disappear也一样），会被调用多次。
* viewDidLoad是在程序启动一次后设置对象，只会被调用一次。

###Cocoa中一些重要的viewcontroller

* UITableViewController(需实现 UITableViewDataSource 协议)
* UINavigationController

后面书中还有很多内容，如自动化布局，GCD，core data等。比较有用的是TableViewCell的个性化设置与GCD吧。改天抽时间再写吧。
这本书的内容到是挺广泛的，就是不太深入。







