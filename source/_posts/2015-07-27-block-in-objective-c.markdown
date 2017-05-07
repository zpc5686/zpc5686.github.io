---
layout: post
title: "block in Objective-C"
date: 2015-07-27 22:09:07 +0800
comments: true
categories: 
---
## Objective-C 中的block的应用


block 语法是从iOS4以后支持的，这项技术在其它语言中已经有类似的东西了，例如C# 中的lambda,python 中的closure等。可以说用block编程是一种大的趋势，现代编程语言中基本上都有匿名函数，我们也经常可以看到函数是一等公民这种说法。

本文侧重于block的应用而非实现，block在底层的实现可以在[这里](http://www.devtang.com/blog/2013/07/28/a-look-inside-blocks/)。

###block的声明 

```bash
void (^plusOne)(int a);
```

这样就声明了一个block，但是每次都这样写太麻烦了，所以我们可以用typedef来定义block

```bash
typedef void(^plusOne)(int);
```

这样就定义了一个叫plusOne的返回值是void，参数是int的block，下次再用的时候就不用写那么长的定义了。

###block的使用

block是可以当做property的，这意味着它是可以被用来当参数传递的， 当定义block的property的时候需要将其属性定义成```copy```，这是因为我们需要将block拷贝一份到堆上。

#####__weak关键字
block对自己内部的对象是strong引用的，所以这里可能会引起retain cycle，所以当我们在block内部使用一个已经包含了该block的对象时候，需在block外面使用__weak关键字，以iOS programming书中的代码为例

```
__weak THItemCell* weakcell = cell;
    cell.actionBlock=^{
        if ([[UIDevice currentDevice] userInterfaceIdiom ]== UIUserInterfaceIdiomPad ) {
            NSString* itemKey = item.itemKey;
            THItemCell *strongCell = weakcell;
            UIImage* img = [[THImageStore shareStore]  imageForKey:itemKey];
            
            if (!img) {
                return ;
            }
            
            CGRect rect = [self.view convertRect:strongCell.thumbnailView.bounds fromView:strongCell.thumbnailView];
            
            THImageViewController *ivc = [[THImageViewController alloc]init  ];
            
            ivc.image = img;
            
            self.imagePopover = [[UIPopoverController alloc] initWithContentViewController:ivc];
            self.imagePopover.delegate = self;
            self.imagePopover.popoverContentSize = CGSizeMake(600, 600);
            [self.imagePopover presentPopoverFromRect:rect inView:self.view permittedArrowDirections:UIPopoverArrowDirectionAny animated:YES];
        }
    };

```
可以看到在block中引用了cell，这时就需要在外部用weak关键字声明一下，以避免retain cycle。

##### __block关键字
在block内引用局部变量时，block自身是对变量的拷贝进行操作的，即使在block内部改变也是改变的变量的拷贝。如果在block内部修改了变量，编译器会报错。这时就需要在block外部使用__block关键字去修饰要改变的变量。


##### block的使用场景
一般来讲有两种使用场景：

1. sdk 提供的很多函数中有completeHandler这种匿名调用的方式，在使用gcd时候也是属于这样的情况的。
2. block当做property进行传递的。按照Single Responsibility的设计原则，一个类A应该负责其单一的功能，而某一功能的实现需要其他类B实现，这时可以在类A中定义一个block属性，将该property在类B中实现，然后在类A中，直接调用改block即可。
