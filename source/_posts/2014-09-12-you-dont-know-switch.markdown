---
layout: post
title: "You dont know switch"
date: 2014-09-12 23:21:44 +0800
comments: true
categories: 
---
## 你可能不知道的switch ##

switch语句是一个常用的分支语句，可以让代码更加直观。其实switch的功能是可以被`if else`替代的。使用switch可以让代码的结构更加清晰，就像新的swift 2.0中加入`guard`一样。为了增加代码的可读性。

不过switch有时候也会有点调皮，比如下面的代码

```bash
void foo(int a)
{
          switch(a)
          {
            case 1:
            printf("case 1\n");
            break;
            if(a>3)
            {
              case 2:
              printf("case 2\n");
              break;
            }
            default :
            printf("default\n");
            break;
          }
}
int main()
{
    foo(2);
    return 0;
}
```
在Ubuntu下用clang和gcc分别编译后，得到的结果都一样，都是case 2

虽然上面这个代码片段逻辑有问题，但是可以成功地说明，switch的本质就是对条件的匹配！在它的内部是不能做if判断的。
