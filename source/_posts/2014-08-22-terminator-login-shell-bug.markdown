---
layout: post
title: "Terminator Login Shell Bug"
date: 2014-08-22 00:14:15 +0800
comments: true
categories: 
---
## 先说点闲话

这个域名已经申请了一年了，最开始是准备用来记录毕业论文的撰写流程的。
当时的想法很简单，就是每天编程，然后晚上将白天编程过程中遇到的问题记录下来用博客的形式记录下来，这样写论文的时候也不至于没有东西可以写吧。
可惜没有坚持下来。。。这也是我最近在改正的一个毛病 **想法多，实现少。** 或者说是 **说的多，做的少** 。
下面来写一下今天用Terminator这个终端神器时候遇到的问题吧。

## Terminator 的bug

Terminator的强大之处已经有很多人介绍过了。我比较喜欢它可以全屏使用，就像是在文字登陆的Linux上操作。
非常带感。当然这不是重点，重点是我今天用RVM指定ruby版本的时候，被提示到：

我设置完了以后，重新登陆Terminator结果发现没有变啊，RVM还是不能USE。
然后我想可能是Terminator启动的时候读配置文件会覆盖吧，于是找到配置文件发现没有相关变量。
我想不会是个bug吧。于是Google，发现讨论组里已经有很多人遇到了，而且没有解决。不过针对我这个问题有个workaround。就是在.bashrc中添加

```bash
[[ -s ${HOME}/.rvm/scripts/rvm ]] && source ${HOME}/.rvm/scripts/rvm
```
