---
layout: post
title: "oci problem"
date: 2015-04-13 10:28:42 +0800
comments: true
categories: 
---
### 问题

今天多拨程序出现了一个奇怪的问题。在Linux下用c语言对Oracle数据库做update操作的时候，执行几条还行，再多执行几条就会卡住。整个程序就像是死掉一样，没有日志打印，也不再执行任何操作。

###解决

经过调试发现，程序是卡在了

```bash
        m_queryResult = OCIStmtExecute(m_serviceContextHandle, m_stmtHandle, m_errHandle,

        1, 0, (OCISnapshot *)0, (OCISnapshot *)0, OCI_DEFAULT);
```
这条语句上。这个函数是oci自己的接口，而且底层的Oracle驱动公司已经用了好几年了，不会出错啊。
        
- 问谷歌：
由于公司没有VPN，我用ggss.so这个谷歌服务镜像，搜索无果。                                 

- 问sof。
sof上关于这个函数的问题，只有5页，看完发现没有类似的问题。
- 看官方文档。
前两个都没有只能去看oci的官方文档了，官方文档好多啊，[这里](http://docs.oracle.com/cd/E11882_01/appdev.112/e10646/oci04sql.htm#LNOCI040)可以看到。看官方的例子发现最后一个参数用的是
```bash
OCI_BATCH_ERRORS 
```
难道是这里出错了吗？改成这个mode后，运行程序，一切正常。
###under the hood

什么情况下才用```OCI_BATCH_ERRORS``` 这个呢？，以下是官方解答

```bash

OCI provides the ability to perform array DML operations.

For example, an application can process an array of INSERT, UPDATE, or DELETE statements with a 
single statement execution. If one of the operations fails due to an error from the server, 
such as a unique constraint violation, the array operation terminates, and OCI returns an error. 
Any rows remaining in the array are ignored. The application must then reexecute the remainder of 
the array, and go through the whole process again if it encounters more errors, which causes additional round-trips.

```

这里说的是，如果使用array进行 update insert 或者 delete操作时，需要使用```OCI_BATCH_ERRORS```这个mode，但是我们这里是一条update，为什么也要用呢？这个我也不清楚，不过按照官方的例子改了以后，程序确实不会卡死了。。。
