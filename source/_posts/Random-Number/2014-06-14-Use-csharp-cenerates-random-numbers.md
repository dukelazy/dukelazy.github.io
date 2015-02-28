---
Language: zh_CN
Author: Shepherd Lazy
#Email: shepherd.lazy@Gmail.com
Create_by: Shepherd Lazy <shepherd.lazy@Gmail.com>
Date_Time: 2014-6-14 20:50:47
layout: post
title: C#生成随机数
Subtitle: 
categories: 
tags: 
Description:  
---

### 导语引言 ###

### 随机数说明 ###
.net.Frameword中提供了一个专门产生随机数的类System.Random
### 随机数生成器基本使用 ###

我们可以使用两种方式初始化一个随机数发生器,
 微软的随机数生成器为我们提供了两种方式初始化一个随机数生成器。一种是默认的构造函数`Random generator = new Random();`，用该方法初始化的生成器.net.Frameword使用与时间相关的默认种子值为我们创建出一个随机数生成器；第二种是使用指定的随机种子值初始化一个随机生成器；

```
Random ro = new Random(10);
```

或

``` csharp
Random ra = new Random(unchecked((int)DateTime.Now.Ticks));
```
```csharp
long tick = DateTime.Now.Ticks;
Random ran = new Random((int)(DateTime.Now.Ticks & 0xffffffffL) | (int)(tick >> 32));
```

或

```csharp
Random ro = new Random(iSeed());
static int iSeed() //有返回值，叫子函数（ＶＢ），无返回值，叫子程序（ＶＢ）；；；Ｃ#中，统一叫方法.
{
    //一个子方法，跟据tick产生随机数的种子
    long tick = 987654123;//定义一干扰常数，9位数且尾数不为0，自定
    tick = tick * (System.DateTime.Now.Millisecond + System.DateTime.Now.Month);//乘以毫秒为干扰
    tick = tick + System.DateTime.Now.Ticks; //加上tick，主变量
    tick = tick + System.Environment.TickCount * (System.DateTime.Now.Millisecond + System.DateTime.Now.Year);//开机毫秒数
    tick = tick + System.Environment.WorkingSet * (System.DateTime.Now.Millisecond + System.DateTime.Now.Day);//内存干扰
    string sTick = tick.ToString();//将tick转换为字符;
    Console.WriteLine("转换为字符的64位Long " + sTick);
    string sSeed = sTick.Substring(sTick.Length - 10); //先取10位
    if (Convert.ToDouble(sSeed) > 2147483642) // 聪明不可用尽，int32亦不取尽
    {
        sSeed = sTick.Substring(sTick.Length - 9); //溢出int32正数，则取9位
    }
    return Convert.ToInt32(sSeed); //转换为int32数字，把算出的数，给回去iSeed，呵呵~~
    //一个子方法，跟据tick产生随机数的种子
}
```
```csharp
static int GetRandomSeed()
{
    byte[] bytes = new byte[4];
    System.Security.Cryptography.RNGCryptoServiceProvider rng = new System.Security.Cryptography.RNGCryptoServiceProvider();
    rng.GetBytes(bytes);
    return BitConverter.ToInt32(bytes, 0);
}
Random random = new Random(GetRandomSeed());
```

#### 参考文档
- *红叶菩提. [C# 随机数种子的获取](http://onetown.blog.163.com/blog/static/9025535201311520533/    "C# 随机数种子的获取"). 网易博客. 2013-02-01 [2014-6-15].*
- *旧语新思. [C#最佳随机数生成（Mersenne Twister）及最佳种子获得方法](http://growupsoft.blog.163.com/blog/static/960729201092611427366/ "C#最佳随机数生成（Mersenne Twister）及最佳种子获得方法  "). 网易博客. 2010-10-26 [2014-6-15].*
- *许明吉. [C# 生成随机数](http://www.cnblogs.com/jxsoft/archive/2011/03/15/1984509.html "C# 生成随机数"). 博客园. 2011-03-15 [2014-6-15].*
- *amber(ms). [C#生成随机数的三种方法](http://www.cnblogs.com/izanami/archive/2011/04/20/2022173.html "C#生成随机数的三种方法"). 博客园. 2011-04-20 [2014-6-15].*
- *XCQ1228. [C#中的随机数种子 ](http://blog.csdn.net/XCQ1228/article/details/2771287 "C#中的随机数种子 "). CSDN博客. 2008-08-05 [2014-6-15].*


