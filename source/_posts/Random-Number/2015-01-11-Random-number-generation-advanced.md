---
Language: zh_CN
Author: Shepherd Lazy
#Email: shepherd.lazy@Gmail.com
Create_by: Shepherd Lazy <shepherd.lazy@Gmail.com>
Date_Time: 2015-1-11 11:28:35 
layout: post
title: 生成随机数进阶
Subtitle: 
categories: 
tags: 
Description:  
---

### 生成随机数进阶 ###

- 不指定随机数生成范围
- 指定随机数生成范围
- 生成随机的双精度浮点数

### 生成不重复随机数 ###

### 按概率生成随机数 ###

### 参考文档 ###

我们在做能自动生成试卷的考试系统时，常常需要随机生成一组不重复的题目，在.net Framework中提供了一个专门用来产生随机数的类System.Random。

对于随机数，大家都知道，计算机不可能产生完全随机的数字，所谓的随机数发生器都是通过一定的算法对事先选定的随机种子做复杂的运算，用产生的结果来近似的模拟完全随机数，这种随机数被称 作伪随机数。伪随机数是以相同的概率从一组有限的数字中选取的。所选数字并不具有完全的随机性，但是从实用的角度而言，其随机程度已足够了。伪随机数的选 择是从随机种子开始的，所以为了保证每次得到的伪随机数都足够地“随机”，随机种子的选择就显得非常重要。如果随机种子一样，那么同一个随机数发生器产生 的随机数也会一样。一般地，我们使用同系统时间有关的参数作为随机种子，这也是.net Framework中的随机数发生器默认采用的方法。

### 初始化种子发生器 ###
我们可以使用两种方式初始化一个随机数发生器：
- 不指定随机种子，系统自动选取当前时间作为随机种子：

```csharp
Random ro = new Random();
```
- 可以指定一个int型参数作为随机种子，这样可以保证99%不是一样。之后，我们就可以使用这个Random类的对象来产生随机数，这时候要用到Random.Next()方法。这个方法使用相当灵活，你甚至可以指定产生的随机数的上下限。

```csharp
int iSeed = 10;
Random ro = new Random(10);
long tick = DateTime.Now.Ticks;
Random ran = new Random((int)(DateTime.Now.Ticks & 0xffffffffL) | (int)(tick >> 32));
```

### 生成随机数 ###

1. 不指定上下限的使用如下：

```C#
int iResult;
iResult=ro.Next();
```
2. 下面的代码指定返回小于100的随机数：
```C#
int iResult;
int iUp=100;
iResult=ro.Next(iUp);
```
3. 而下面这段代码则指定返回值必须在50-100的范围之内：
```C#
int iResult;
int iUp=100;
int iDown=50;
iResult=ro.Next(iDown,iUp);
```
4. 除了Random.Next()方法之外，Random类还提供了Random.NextDouble()方法产生一个范围在0.0-1.0之间的随机的双精度浮点数：

```C#
double dResult;
dResult=ro.NextDouble();
```

## 解决生成随机数重复 ##
但是用Random类生成题号，会出现重复，特别是在数量较小的题目中要生成不重复的的题目是很难的，参考了网上的一些方法，包括两类，一类是通过随机种子入手，使每一次的随机种子不同，来保证不重复；第二类是使用一些数据结构和算法。下面主要就第二类介绍几个方法。
　 　
- **方法一**
思想是用一个数组来保存索引号，先随机生成一个数组位置，然后把这个位置的索引号取出来，并把最后一个索引号复制到当前的数组位置，然后使随机 数的上限减一，具体如：先把这100个数放在一个数组内，每次随机取一个位置（第一次是1-100，第二次是1-99，...），将该位置的数用最后的数 代替。

```csharp
    int[] index = new int[15];
    for (int i = 0; i < 15; i++)
        index = i;
    Random r = new Random();
    //用来保存随机生成的不重复的10个数
    int[] result = new int[10];
    int site = 15;//设置下限
    int id;
    for (int j = 0; j < 10; j++)
    {
        id = r.Next(1, site - 1);
        //在随机位置取出一个数，保存到结果数组
        result[j] = index[id];
        //最后一个数复制到当前位置
        index[id] = index[site - 1];
        //位置的下限减少一
        site--;
    } 
```


- **方法二**
利用Hashtable。

```C#
Hashtable hashtable = new Hashtable();
Random rm = new Random();
int RmNum = 10;
for (int i = 0; hashtable.Count < RmNum; i++)
{
    int nValue = rm.Next(100);
    if (!hashtable.ContainsValue(nValue) && nValue != 0)
    {
        hashtable.Add(nValue, nValue);
        Console.WriteLine(nValue.ToString());
    }
} 
```

- **方法三**
递归，用它来检测生成的随机数是否有重复，如果取出来的数字和已取得的数字有重复就重新随机获取。

```C#
Random ra = new Random(unchecked((int)DateTime.Now.Ticks));
int[] arrNum = new int[10];
int tmp = 0;
int minValue = 1;
int maxValue = 10;
for (int i = 0; i < 10; i++)
{
    tmp = ra.Next(minValue, maxValue); //随机取数
    arrNum = getNum(arrNum, tmp, minValue, maxValue, ra); //取出值赋到数组中
}
```
```C#
public int getNum(int[] arrNum, int tmp, int minValue, int maxValue, Random ra)
{
    int n = 0;
    while (n <= arrNum.Length - 1)
    {
        if (arrNum[n] == tmp) //利用循环判断是否有重复
        {
            tmp = ra.Next(minValue, maxValue); //重新随机获取。
            getNum(arrNum, tmp, minValue, maxValue, ra);//递归:如果取出来的数字和已取得的数字有重复就重新随机获取。
        }
        n++;
    }
    return tmp;
}
```

http://www.connectionstrings.com/

##*参考资料*##
- *[deerchao](http://www.unibetter.com/members/deerchao.aspx). [正则表达式30分钟入门 v2.31](http://unibetter.com/deerchao/zhengzhe-biaodashi-jiaocheng-se.htm). 2009-4-11 [2010-04-16]*
- *willpower88. [正则表达式](http://www.blogjava.net/willpower88/archive/2007/04/10/109669.html "正则表达式"). BlogJava. 2007-04-10 [2010-04-16].*
- *kkun_3yue3. [提取网页正文内容代码](http://bbs.csdn.net/topics/110062075#post-110555919). CSDN论坛. 2007-05-11 [2010-04-16].*    
- *gradylu. [如何提取网页上的内容（C#） ](http://blog.csdn.net/tigerlgf/article/details/1529989). CSDN博客. 2007-03-15 [2010-04-16].*  
- *Kevin Yang. [使用正则表达式匹配嵌套Html标签](http://www.cnblogs.com/KevinYang/archive/2009/08/04/1538050.html). 博客园. 2009-08-04 [2010-04-16]*
- *Laser_Lu. [可以解析HTML/XHTML页面的所有元素和结构的Regular Expression！](http://www.cnblogs.com/Laser_Lu/archive/2005/04/21/142605.html). 博客园. 2005-04-21 [2010-04-16]*
- *wuhuiran. [正则表达式提取HTML页面的特定部分 ](http://blog.csdn.net/wuhuiran/article/details/2750765). CSDN博客. 2007-03-15 [2010-04-16].*
- *tgw2000. [正则表达式](http://tgw2000.blog.163.com/blog/static/68159568200851210260475/). 网易博客 2008-06-12 [2010-04-16].*
- *byxdaz. [常用正则表达式大全](http://blog.csdn.net/byxdaz/article/details/4470684). CSDN博客 2009-08-21 [2010-04-16].*
- *魑魅魍魉. [保存图片的SRC地址](http://hi.baidu.com/mepowbjbrpfuvye/item/689452a28d423f9e1410732f "保存图片的SRC地址"). 百度空间. 2007-11-18[2010-04-16].*
- *感動常在. [利用正则表达式去掉html代码](http://www.cnblogs.com/wang123/archive/2006/09/16/505758.html). 博客园. 2006-09-16 [2010-04-16].*
- *ljpbxh027. [正则表达式用于字符串处理、表单验证等场合，实用高效。现将一些常用的表达式收集于此，以备不时之需。](http://hi.baidu.com/ljpbxh027/item/ef1e705c0191a43594eb0563   "正则表达式用于字符串处理、表单验证等场合，实用高效。现将一些常用的表达式收集于此，以备不时之需。"). 百度空间. 2006-10-18 [2010-04-16].*

- *. []( ""). . .*