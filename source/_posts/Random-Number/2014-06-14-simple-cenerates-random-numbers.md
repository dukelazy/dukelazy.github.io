---
Language: zh_CN
Author: Shepherd Lazy
#Email: shepherd.lazy@Gmail.com
Create_by: Shepherd Lazy <shepherd.lazy@Gmail.com>
Date_Time: 2014-6-14 23:09:19
layout: post
title: 简单生成随机数
Subtitle: 
categories: 
tags: 
Description:  
---

```
using System;

namespace RandomTest
{
    class Program
    {
        static void Main(string[] args)
        {
            for (int i = 0; i < 100; i++)
            {
                Random d = new Random();
                Console.WriteLine(d.Next(100));
            }
        }
    }
}
```
理论上而言，这个程序会产生100个不同的0～100的整数，而实际情况却是除了第一个数字不同外，剩余99个数字会产生随机的99个相同的数字！而在中间加入调试点或者用MessageBox.show()的方式却能正确的得到100个不同的随机数！ 
为什么这样？难道要暂停一下子？于是修改代码： 

```
using System;

namespace RandomTest
{
    class Program
    {
        static void Main(string[] args)
        {
            for (int i = 0; i < 100; i++)
            {
                Random d = new Random();
                Thread.Sleep(15);
                Console.WriteLine(d.Next(100));
            }
        }
    }
}
```
再次运行后，输出的数字终于随机了，而且15毫秒以上的暂停才会正常，如果只暂停1毫秒的话，会规律地出现连续5-6个一样的随机数，如果改成5毫秒的暂停的话，这种重复产生一样随机数的概率变成2-3个！ 
在网上苦苦搜索了2天，没什么帮助，而在CSDN论坛却很快有人给了解决方法： 

```
using System;

namespace RandomTest
{
    class Program
    {
        static void Main(string[] args)
        {
            Random d = new Random();
            for (int i = 0; i < 100; i++)
            {
                Console.WriteLine(d.Next(100));
            }
        }
    }
}

```

把随机对象放在循环的外面就能解决问题！但还是没人能给个解释。估计果然是因为伪随机数的缘故，每次新产生随机种子的时候有时间的参与，所以才会在短时间内产生完全重复一致的“伪随机数”吧！ 
又及：网上看到一个提高随机数不重复概率的种子生成方法。

```
static int GetRandomSeed()
{
    byte[] bytes = new byte[4];
    System.Security.Cryptography.RNGCryptoServiceProvider rng = new System.Security.Cryptography.RNGCryptoServiceProvider();
    rng.GetBytes(bytes);
    return BitConverter.ToInt32(bytes, 0);
}

Random random = new Random(GetRandomSeed());
```
